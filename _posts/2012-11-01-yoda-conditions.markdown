---
layout: post
title:  "요다 조건문"
categories: programming etc
date:   2012-11-01 21:14:37
tags:   yoda conditions
---

요다 조건문이란?

조건문 작성시 일반적인 방식과는 다르게 변수를 오른쪽에 쓰고, 상수를 왼쪽에 쓰는 코드 작성법을 말한다.

{% highlight C++ %}
int i = 10;
if ( 10 == i )   // Yoda Conditions
  // Do something
{% endhighlight %}

이렇게 쓰게 된다면 실수로 등호를 한개만 작성했을때 컴파일 에러가 발생하므로, 오류를 쉽게 찾을수 있다는 장점이 있다.

{% highlight C++ %}
int i = 10;
if ( 10 = i )  // Error!!
  // Do something
{% endhighlight %}

이와같은 형태를 요다 조건문이라고 부르는 이유는 스타워즈의 주인공중 한명인 요다가 실제로 말할때 [어순을 바꿔서 말하기 때문](http://mirror.enha.kr/wiki/%EC%9A%94%EB%8B%A4#s-1)이다.