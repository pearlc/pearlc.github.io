---
layout: post
title:  "PHP에서 큰 따옴표와 작은 따옴표의 차이점"
categories: programming php
date:   2012-11-01 20:43:20
tags: php, single-quote, double-quote, 작은따옴표, 큰따옴표, 차이
---

php 작성시 문자열을 나타내고자 할때는 큰따옴표나 작은따옴표를 써서 해결할수 있다.

{% highlight php %}
<?php
$abc = "abb";
function testFunction () {
    echo 'hi';
    return $dd;
}
print "double quote"; // double quote
print 'single quote'; // single quote
{% endhighlight %}

이 예제에서는 큰따옴표와 작은따옴표의 기능이 동일하지만, 상황에 따라 이 두개는 다르게 동작할수도 있다.

예를들어 큰따옴표는 내부 문자열을 파싱을 해서 뿌려주는 반면에 작은따옴표는 내부 내용을 그대로 출력하게 된다.

즉, 아래와 같이 문자열 내부에 변수를 사용하면 큰따옴표는 그 내용을 변수값으로 치환해서 보여준다.

{% highlight php %}
<?php
$a = "Hello world";
print "double quote : $a"; // double quote : Hello world
print 'single quote : $a'; // single quote : $a
{% endhighlight %}

또한 개행문자 "\n" 도 큰따옴표 에서는 실제 개행문자로 변환을 하는 반면에 작은따옴표에서는 "\n" 문자열을 그대로 출력한다.

{% highlight php %}
<?php
print "double quote : \n"; // double quote :
print 'single quote : \n'; // single quote : \n
{% endhighlight %}

이런 이유로 인해 일부 개발자들은 큰따옴표보다 작은따옴표의 처리속도가 더 빠르다고는 하지만 실제로 그 효과는 미미하다고 볼수 있다.