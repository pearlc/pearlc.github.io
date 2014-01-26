---
layout: post
title:  "여러 도메인들 간 쿠키 공유하기"
categories: programming etc
date:   2011-11-13 22:30:13
tags:   sharing-cookies-across-multiple-domains-hosted-on-different-servers
---

도메인들 간 쿠키 공유하기

(Sharing cookies across multiple domains hosted on different servers)

이 글은 <http://www.innovativephp.com/sharing-cookies-across-multiple-domains-hosted-on-different-servers/> 의 글을 번역한 글입니다.

---

### 도메인들간 쿠키 공유에 대한 간단한 소개

#### 왜 필요한가?

웹사이트나 웹어플리케이션을 개발할때 필요에 따라 단일 도메인이나 별개의 도메인 또는 서브도메인을 사용할수 있다. 그리고 일반적으로 보안이나 성능, 서버부하 등에 대한 이유로 서브 도메인 형식으로 구성한다

구글은 이 개념을 이해하는데에 좋은 예이다. 구글은 메일, 검색, 구글+ 등의 서비스에 대해 서브도메인 형식으로 서비스를 제공한다. 구글은 자신들의 서비스에 단일 로그인 정보를 사용하기 때문에 서비스 간에 로그인 정보를 공유할수 있는 방법이 필요하다. 보통 로그인 데이터는 브라우저 세션에 저장된다. 이때 각 서브도메인은 별개의 세션을 유지하기 때문에 세션 기능을 이용하는한 로그인 정보를 공유하는것은 불가능하다. 바로 이점이 쿠키가 필요한 이유다. 쿠키값은 여러 사이트에서 접근할수 있으며 각각의 어플리케이션에 필요한 정보를 유지할수 있다. PHP는 도메인들간 쿠키를 공유하는 기능을 지원한다

#### 도메인 간 쿠키 공유의 제한점

비록 도메인 간에 쿠키값을 저장하고 접근하는것이 가능하긴 하지만 몇가지 제한사항이 있다. 다른 도메인의 쿠키값은 저장할수 없다는 것이다. 단지 동일한 사이트의 서브도메인 쿠키를 이용할수 있을 뿐이다.

동일한 서버에서 서비스되는 www.example1.com 와 www.example2.com 이라는 사이트가 있다고 가정하자. 이 두개는 같은 서버에 있기는 하지만 도메인 이름이 다르다. 따라서 이 두 사이트간 쿠키를 공유할 수는 없다. 하지만 www.example.com 과 이 사이트의 서브도메인을 활용한 사이트, 예를들어 app.example.com, demo.example.com 를 사용한다면 이 세 도메인간의 쿠키는 공유될수 있다. 이때 이 서비스가 반드시 동일한 서버에 있을 필요는 없으며, 모든 도메인이 하나의 top domain에 속할 필요는 없다.

---

### PHP로  cookie 설정하기/가져오기

#### PHP 를 사용해서 쿠키 저장하기

php는 웹브라우저상에서 쿠키를 저장하고 삭제할수 있는 함수 setCookie 를 기본적으로 제공한다

옵션에 따라 다양한 환경에서의 사용하는것도 가능하다

{% highlight php %}
setcookie(cokie name, cookie value, cookie lifetime, cookie path, domain, connection type, http access);
{% endhighlight %}

- Cookie Name – 쿠키 이름. 한번 설정하면 이후로는 $_COOKIE['name'] 로 접근할수 있다.
- Cookie Value – 쿠키의 값.
- Cookie Lifetime – 만료나 삭제까지 남은 시간. 보통 time()+3600 (현재시각+한시간) 과 같은 형식으로 사용한다 
- Cookie Path – 웹사이트의 특정 디렉토리 내에서만 유효하도록 설정하는 변수. "/" 로 설정하면 웹사이트 전체에서 접근할수 있다
- Cookie Domain – 특정 서브도메인 또는 도메인 전체를 유효범위로 지정하는 변수.
- Connection Type – TRUE 또는 FALSE 값을 가진다. TRUE일 경우 HTTPS인 경우에만 접근 가능하다.
- HTTP Access – TRUE 또는 FALSE 값을 가진다. TRUE일 경우에는 HTTP로 접근할 시에만 접근 가능하다

#### 쿠키 값 가져오기

{% highlight php %}
$_COOKIE['cookie_name'];
{% endhighlight %}

#### 쿠키 삭제하기

쿠키 삭제 함수는 쿠키 저장 함수와 똑같다. 단지 Lifetime 파라미터를 과거 시점의 값으로 설정해주면 된다. 

생성을 아래와 같이 했다면

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/");
{% endhighlight %}

아래와 같이 삭제할 수 있다
{% highlight php %}
setcookie("Logged", "True", time()-3600, "/");
{% endhighlight %}

---

### 쿠키 함수의 올바른 사용예

#### 이름과 값으로 생성

{% highlight php %}
setcookie("Logged", "True", time()+3600);
{% endhighlight %}

#### 어플리케이션 전체를 범위로 생성

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/");
{% endhighlight %}

#### 어플리케이션 내의 특정 폴더를 범위로 생성

아래 함수는 어플리케이션 내에서 /blog/tutorials/ 폴더 내에서만 작동한다

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/blog/tutorials/");
{% endhighlight %}

#### 도메인 간 공유 쿠키 생성

아래 함수는 exmaple 도메인과 그 하위 도메인에 대해서만 작동한다

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/", ".example.com");
{% endhighlight %}

#### 특정 하위도메인에 대해서만 생성

아래 함수는 app.example.com 도메인에 대해서만 작동한다

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/", ".app.example.com");
{% endhighlight %}

#### 보안이 유지되는 특정 하위 도메인의 쿠키로 생성

아래 함수는 example.com과 그 하위 도메인 영역에서 https 연결하고 있을 경우에만 작동한다

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/", ".example.com", 1);
{% endhighlight %}

#### 보안이 유지되는 특정 하위 도메인의 쿠키로 생성. HTTP 요청으로만 접근 가능

아래 함수는 example.com과 그 하위 도메인 영역에서 https 연결하고 있을 경우에 작동하며 HTTP(이 경우에는 HTTPS) 연결을 통해서만 접근이 가능하다

{% highlight php %}
setcookie("Logged", "True", time()+3600, "/", ".example.com", 1, 1);
{% endhighlight %}
