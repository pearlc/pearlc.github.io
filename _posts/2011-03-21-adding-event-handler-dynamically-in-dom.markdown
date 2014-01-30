---
layout: post
title:  "DOM 객체에 동적으로 이벤트 핸들러 추가하는 방법"
categories: programming javascript
date:   2011-03-21 00:49:28
tags:   javascript,jquery,dom,event,handler,dynamic,bind,live, delegate, on, 이벤트, 핸들러, 동적, 제이쿼리, 자바스크립트
alias:  /index.php/2011/03/21/html-%EA%B0%9D%EC%B2%B4%EC%97%90-%EB%8F%99%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%95%B8%EB%93%A4%EB%9F%AC-%EC%B6%94%EA%B0%80%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95/
---

jQuery를 이용해서 동적으로 이벤트 핸들러를 추가하기 위해서 `bind()`, `live()`, `delegate()` 과 같은 함수를 사용할수 있다

가장 기본적인것은 `bind()` 함수로

{% highlight html %}
<body>
  <div>
    Click here
  </div>
</body>
{% endhighlight %}

위와 같은 html 문서가 있을때

{% highlight javascript %}
$('.clickme').bind('click', function() {
  // Bound handler called.
});
{% endhighlight %} 

이렇게 해서 clickme 클래스를 가진 객체에 대해 onclick 이벤트를 처리할수 있다

하지만 이후에 아래의 코드가 실행되는 상황을 가정해 보자
 
{% highlight javascript %}
$('body').append('<div>Another target</div>');
{% endhighlight %}

그렇다면 새로 추가되는 div 객체 또한 clickme 클래스인데, 이 클래스의 경우에는 앞서 추가한 onclick 이벤트 핸들러가 등록 되어 있지 않다.

이러한 경우에 “나중에 추가될 객체” 까지 고려해서 bind를 해주는 함수가 `live()` 이다

`live()`함수는 위와 같이 `bind()`의 발전된 버전이며 jquery 버전이 1.3 이상일 경우에 사용이 가능하다

{% highlight javascript %}
$('.clickme').live('click', function() {
  // Live handler called.
});
{% endhighlight %}

이렇게 사용한다면, `append()` 를 통해 새로운 clickme 클래스 객체 추가된다고 하더라도 onclick 이벤트 핸들러가 작동하게 된다

마지막으로 `delegate()` 는 조건에 해당하는 모든 객체에 이벤트 핸들러를 연결시킨다

{% highlight javascript %}
$("table").delegate("td", "hover", function(){
	$(this).toggleClass("hover");
});
{% endhighlight %} 

위와 같이 사용하면, &lt;table&gt; 아래에 있는 모든 &lt;td&gt; 들은 hover 핸들러를 가지게 된다

이것을 `live()`를 사용해서 구현하면 아래와 같으며, 이 둘은 동일하다

{% highlight javascript %}
$("table").each(function(){
	$("td", this).live("hover", function(){
		$(this).toggleClass("hover");
	});
});
{% endhighlight %}