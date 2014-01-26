---
layout: post
title:  "jQuery와 javascript 에서의 trim() 함수"
categories: programming javascript
date:   2011-03-21 00:46:23
tags:   jquery, javascript, trim
---

대부분의 프로그래밍 언어에서 `trim()` 함수는 문자열 앞과 뒤 공백을 제거하는 함수이다. 하지만 javascript 에서는 기본적으로 이 함수를 제공하지 않는데, 이 때문에 jquery의 `trim()`이 많이 사용된다

{% highlight javascript %}
<script type="text/javascript" src="http://code.jquery.com/jquery-1.4.4.min.js"></script>
<script>
    var str = ...;
	var trimmed_str = jQuery.trim(str);
</script>
{% endhighlight %}
 만약 jquery를 쓰기 싫다면 정규식을 사용한 아래 함수를 이용하면 된다

{% highlight javascript %}
<script type="text/javascript">
function trim(stringToTrim) {
	return stringToTrim.replace(/^\s+|\s+$/g,"");
}
var trimmed_str = trim(str);
</script>
{% endhighlight %}