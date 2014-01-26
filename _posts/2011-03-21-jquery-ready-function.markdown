---
layout: post
title:  "window.onload를 대체하는 jquery의 ready 함수"
categories: programming javascript
date:   2011-03-21 00:41:39
tags:   javascript onload jquery ready
---

window.onload 함수는 웹페이지의 로딩이 끝나는 시점에 실행되는 함수다

{% highlight javascript %}
window.onload = function () {
 alert("로딩 완료");
}
{% endhighlight %}
와 같이 사용하게 되면 페이지 로드가 끝난 후에 "로딩 완료" 라는 얼럿창이 뜨게 된다

하지만 onload에는 몇가지 단점이 있는데, 이 함수는 페이지 안의 이미지나 외부 파일이 로드 될때까지도 기다린 후에 사용되기 때문에 엔드유저 입장에서 불필요하게 로딩시간이 길어지게 된다. 더욱 심각한것은 동일한 페이지 내에서 onload 함수는 하나만 존재해야 한다는 것인데, 만약 외부 라이브러리에서 onload가 이미 선언 되어 있다면 이를 찾거나 하나로 합치는 것은 여간 어려운일이 아니다.

또한 `<body onload="">` 와 같은 attribute 가 설정이 되어 있는 경우에는 attribute의 `onload=""`만 작동할뿐 `window.onload`는 동작 하지 않는다

jquery 에서는 이러한 onload의 단점들을 보완하는 ready() 함수를 제공하는데 위의 코드를 jquery 형식으로 바꾸면 다음과 같이 된다

{% highlight javascript %}
<script src="http://code.jquery.com/jquery-1.4.4.min.js"></script>
<script>
$(document).ready(function() { alert("로딩 완료"); });
</script>
{% endhighlight %}
이 ready() 함수가 실행되는 시점은 브라우저가 DOM트리 생성한 직후 이므로 유저 입장에서는 (이미지나 외부 리소스의 로딩을 기다릴 필요 없이)훨씬 빠르게 웹페이지의 기능을 사용할수가 있다

아래는 확인코드, 이 스크립트를 실행하게 되면 `첫번째 ready()` -> `두번째 ready()` -> `두번째 window.onload` 의 순서로 얼럿창이 뜨게된다. (`첫번째 window.onload` 는 실행되지 않는다)

{% highlight javascript %}
<script type="text/javascript" src="http://code.jquery.com/jquery-1.4.4.min.js"></script>
<script type="text/javascript">
window.onload = function() { alert("첫번째 window.onload"); };
window.onload = function() { alert("두번째 window.onload"); };
$(document).ready(function() { alert("첫번째 ready()"); });
$(document).ready(function() { alert("두번째 ready()"); });
</script>
{% endhighlight %}