---
layout: post
title:  "php로 웹상의 파일 읽기(원격 리소스 읽기)"
categories: programming php
date:   2011-07-16 18:08:26
tags:   curl, fsockopen, file_get_content, fopen, 소켓통신
---

이 글은 <http://www.php-mysql-tutorial.com/wikis/php-tutorial/reading-a-remote-file-using-php.aspx>에 있는 글을 번역한 것입니다

---

php로 웹상(원격)에 있는 파일을 읽는 방법으로는 아래의 4가지 방법을 사용해서 구현이 가능하다

1. `fopen()` 함수 사용
2. `file_get_contents()` 함수 사용
3. cURL 라이브러리 사용
4. php의 소켓통신 함수들을 사용

1번과 2번 방법을 사용하기 위해서는 fopen wrapper 가 사용 가능해야한다. 이 fopen wrapper 파라미터는 php.ini에 정의되어 있으나, `ini_set()`을 사용해서 실행시간에 바꿀수는 없다.

이 두 방법을 쓸수 있는지의 여부는 아래 코드로 확인할수 있다

{% highlight php %}
<?php
if (ini_get('allow_url_fopen') == '1') {
   // fopen() 이나 file_get_contents() 사용
} else {
   // curl 이나 함수 직접 작성
}
{% endhighlight %}

---

### 1. fopen() 함수 사용

`fopen()`을 사용하는 법은 local 파일을 읽는것 만큼 쉽다. 유일하게 다른점은 `fopen()`함수 내에 파일명 대신 URL을 적는다는 것이다. 아래 예제를 보자.

{% highlight php %}
<?php
// 원격 파일을 사용하기 전에 성공적으로 open 되었는지 확인
if ($fp = fopen('http://www.google.com/', 'r')) {
   $content = '';
   // 전부 읽을때까지 계속 읽음
   while ($line = fread($fp, 1024)) {
      $content .= $line;
   }

   // content 사용
   // ...
} else {
   // 파일 open시 에러 발생
}
{% endhighlight %}

위 코드중 while 반복문의 `fread()` 함수는 한 루프 안에서 1024 바이트의 데이터를 읽기 위해 사용된다. 이 코드는 아래와 같이 쓸수도 있다.

{% highlight php %}
<?php
// 원격 파일을 사용하기 전에 성공적으로 open 되었는지 확인
if ($fp = fopen('http://www.google.com/', 'r')) {
   $content = '';
   // 전부 읽을때까지 계속 읽음
   while ($line = fgets($fp, 1024)) {
      $content .= $line;
   }

   // content 사용
   // ...
} else {
   // 파일 open시 에러 발생
}
{% endhighlight %}

`fread()` 대신에 최대 1024바이트의 라인 한줄을 읽는 `fgets()`를 사용했다. 첫번째 코드가 두번째보다 좀더 선호되는 방식이다.

원격에 있는 파일이 300줄 짜리 50KB 파일이라고 생각해보면, 첫번째 코드는 루프가 15번 정도 돌테지만 두번째 코드는 300번의 루프가 실행되야 한다

만약 함수호출 비용과 시간을 고려중이라면 첫번째 방법이 확실히 나은 방법이다

### 2. `file_get_contents()` 함수 사용

가장 간단해서 내가 가장 선호하는 방법이다. 단지 파라메터를 url로 주고 함수를 호출하기만 하면 된다. 한가지 기억해야할 점은 리턴받은 값을 사용하기 전에 error가 리턴 됬는지 확인 먼저 해야한다는 것.

{% highlight php %}
<?php
$content = file_get_contents('http://www.google.com/');
if ($content !== false) {
   // content 사용
} else {
   // error 발생
}
{% endhighlight %}

### 3. cURL 라이브러리 사용

위의 두 방법과는 다르게 CURL을 쓰는 방법은 딱 부러지게 설명하기 힘들다. 이 라이브러리는 (http뿐만 아니라) 다른 프로토콜 간의 연결과 통신하는 데에 매우 유용하기 쓰이긴 하지만 배우는데 시간을 좀 들여야 한다. 또 다른 문제는 모든 web host들(서버)이 이 php 라이브러리를 설치하지는 않았다는것. 따라서 이 방법을 쓰기 전에 해당 라이브러리가 설치되어 있는지 먼저 확인 해야한다

다음은 이 CURL라이브러리를 활용해서 원격 파일을 여는 간단한 예제다

{% highlight php %}
<?php
// curl이 설치 되었는지 확인
if (function_exists('curl_init')) {
   // curl 리소스를 초기화
   $ch = curl_init(); 

   // url을 설정
   curl_setopt($ch, CURLOPT_URL, 'http://www.google.com'); 

   // 헤더는 제외하고 content 만 받음
   curl_setopt($ch, CURLOPT_HEADER, 0); 

   // 응답 값을 브라우저에 표시하지 말고 값을 리턴
   curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 

   // 브라우저처럼 보이기 위해 user agent 사용
   curl_setopt($ch, CURLOPT_USERAGENT, 'Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.5) Gecko/20041107 Firefox/1.0'); 

   $content = curl_exec($ch); 

   // 리소스 해제를 위해 세션 연결 닫음
   curl_close($ch);
} else {
   // curl 라이브러리가 설치 되지 않음. 다른 방법 알아볼 것
}
{% endhighlight %}

몇가지 경우에는 `file_get_contents()`나 `fopen()` 을 쓰는것보다 CURL이 더 빠르다. 이것은 CURL이 기본적으로 압축 프로토콜을 사용하기 때문이다 (예를들면 gzip).

크고 작은 많은 사이트에서 bandwidth를 줄이기 위해 그들 페이지에서 gzip 압축을 사용한다. 이 사이트도 gzip 압축을 사용했고, bandwidth가 절반으로 줄었다. 만약 기다리기 싫어하는 타입이라면 CURL이 가장 적당할것이다

### 4. php의 소켓통신 함수들을 사용

최악의 경우에는 서버의 fopen wrapper 옵션도 꺼져있고, CURL 라이브러리도 인스톨 되지 않았을수도 있다. 이 상황에서는 우리가 쓸 함수를 직접 만들어야 한다.

우리의 함수는 대상 파일의 url 파라미터 한개를 갖는 `getRemoteFile()` 함수로 명명했다. 대략적인 뼈대는 아래와 같다

{% highlight php %}
<?php
function getRemoteFile($url)
{
   // 1. host name과 url path 값을 획득

   // 2. 원격 서버에 접속

   // 3. 파일을 얻기위해 필요한 헤더들을 전송

   // 4. 원격 서버로부터 응답 받음

   // 5. header 부분 걷어냄

   // 6. 파일 content 리턴
}
{% endhighlight %}

url에서 host name과 url path 를 추출하기위해서는 `parse_url()` 함수를 이용하면 된다. 이  함수에 넘겨진 url은 다음 항목들로 분리될 것이다

- scheme
- host
- port
- user
- pass
- path
- query
- fragment
 
예를들면, `http://www.php-mysql-tutorial.com/somepage.php` 은 아래와 같이 리턴된다

{% highlight php %}
<?php
Array
(
    [scheme] => http
    [host] => www.php-mysql-tutorial.com
    [path] => /somepage.php
)
{% endhighlight %}

만약 `http://myusername:mypassword@www.php-mysql-tutorial.com/somepage.php?q=whatsthis#ouch` 라면 아래와 같이 리턴된다

{% highlight php %}
<?php
Array
(
    [scheme] => http
    [host] => www.php-mysql-tutorial.com
    [user] => myusername
    [pass] => mypassword
    [path] => /somepage.php
    [query] => q=whatsthis
    [fragment] => ouch
)
{% endhighlight %}

우리가 관심있는것은 host, port, path, query 값 뿐이다

원격 서버와의 connection을 생성하기는 위해서 `fsockopen()`을 사용한다. 이 함수는 [hostname, port number, error number 포인터, error message 포인터, 시간제한] 5개의 인자를 가지고 있다

{% highlight php %}
<?php
function getRemoteFile($url)
{
   // host name 과 url path 값을 획득
   $parsedUrl = parse_url($url);
   $host = $parsedUrl['host'];
   if (isset($parsedUrl['path'])) {
      $path = $parsedUrl['path'];
   } else {
      // url이 http://www.mysite.com 같은 형식이라면
      $path = '/';
   }

   if (isset($parsedUrl['query'])) {
      $path .= '?' . $parsedUrl['query'];
   } 

   if (isset($parsedUrl['port'])) {
      $port = $parsedUrl['port'];
   } else {
      // 대부분의 사이트들은 80포트를 사용
      $port = '80';
   }

   $timeout = 10;
   $response = '';
   // 원격 서버에 접속한다
   $fp = @fsockopen($host, $port, $errno, $errstr, $timeout );

   if( !$fp ) {
      echo "Cannot retrieve $url";
   } else {
      // 필요한 헤더들 전송
      fputs($fp, "GET $path HTTP/1.0\r\n" .
                 "Host: $host\r\n" .
                 "User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.8.0.3) Gecko/20060426 Firefox/1.5.0.3\r\n" .
                 "Accept: */*\r\n" .
                 "Accept-Language: en-us,en;q=0.5\r\n" .
                 "Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7\r\n" .
                 "Keep-Alive: 300\r\n" .
                 "Connection: keep-alive\r\n" .
                 "Referer: http://$host\r\n\r\n");

      // 원격 서버로부터 response 받음
      while ( $line = fread( $fp, 4096 ) ) {
         $response .= $line;
      }

      fclose( $fp );

      // header 부분 걷어냄
      $pos      = strpos($response, "\r\n\r\n");
      $response = substr($response, $pos + 4);
   }

   // 파일의 content 리턴
   return $response;
}
{% endhighlight %}

위의 코드에서는 9줄의 헤더 정보를 보내지만 사실 처음 2줄만 필수사항이다. 따라서 이렇게만 보내도 된다

{% highlight php %}
<?php
fputs($fp, "GET $path HTTP/1.0\r\n" . "Host: $host\r\n\r\n");
{% endhighlight %}
           
아마 대부분의 경우 잘 동작할테지만 항상 잘 동작하는것은 아니다. 열고자 하는 파일들은 원격 서버에 저장되어 있기때문에, 원격 서버가 request에 response 하는지 안하는지에 달려있다

몇몇 서버는 request 헤더에 referer 항목이 없다면 block 할것이고, 몇몇은 특정 user agent 만 받아들일 것이다. 또 어떤 것들은 cookie가 설정되어 있는 header만 받을 것이다

특정 원격 파일을 여는데에 어떤 헤더가 보내져야 하는지 알고 싶다면 파이어폭스와 live http headers plugin 툴을 사용해 보라. 작고 강한 툴이다