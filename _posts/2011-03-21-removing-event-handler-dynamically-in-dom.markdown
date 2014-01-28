---
layout: post
title:  "DOM 객체에 동적으로 추가된 이벤트 핸들러 제거하는 방법"
categories: programming javascript
date:   2011-03-21 00:50:34
tags:   javascript, jquery, event,handler, unbind, die, undelegate, off, 자바스크립트, 제이쿼리, 이벤트, 핸들러, 제거
---

jQuery의 `bind()`, `live()`, `delegate()` 함수를 사용해서 이벤트 핸들러를 등록하는 경우, 각각 `unbind()`, `die()`, `undelegate()` 함수를 사용해서 이벤트 핸들러를 삭제할수 있다.

이때, `bind()`로 추가한것은 반드시 `unbind()`로, `live()` 로 추가한것은 `die()`로, `delegate()`로 추가한것은 `undelegate()` 로 삭제해야 하는것에 주의하자.

이렇게 `unbind()`류의 함수를 사용해서 이벤트를 없앨때에는

{% highlight javascript %}
$('#foo').unbind('click');
{% endhighlight %}

와 같이 사용하면 되며, `unbind()`의 인자를 주지 않으면 `$(‘#foo’)`에 걸려있는 모든 bind된 이벤트핸들러를 삭제하게 된다.

또한 아래와 같이 이벤트 핸들러를 명명해서 bind한 후에 해당하는 핸들러만 없애는 것도 가능하다.
 
{% highlight javascript %}
var handler = function() {
  alert('The quick brown fox jumps over the lazy dog.');
};
$('#foo').bind('click', handler);
$('#foo').unbind('click', handler);
{% endhighlight %}

하지만 아래와 같이 이름을 지정하지 않고 bind한 경우는 동작하지 않는다.

{% highlight javascript %}
$('#foo').bind('click', function() {
  alert('The quick brown fox jumps over the lazy dog.');
});

// will NOT work
$('#foo').unbind('click', function() {
  alert('The quick brown fox jumps over the lazy dog.');
});
{% endhighlight %}

한편, `<td onclick="">` 와 같이 정적으로 onclick이 설정 된것은 `unbind()`로 해제할수 없는데, 이 경우에는 `$(‘.td’).removeAttr(“onclick”);` 와 같이 해제해야한다.