---
layout: post
title:  "브라우저의 content type 인식 문제"
categories: programming etc
date:   2011-06-25 13:40:55
tags:   browser, xml, content-type-header, text/xml, mimetype, 브라우저, 컨텐트타입, mime타입
---

온라인 상에 있는 xml 파일을 url로 열때, 일부 브라우저에서 아래 화면과 같이 xml 태그가 html 태그로 해석되어 나타나는 경우가 있다

![html 파싱 이미지](/assets/uploads/xml-before.png "html entities")

이런 경우에는 xml 파일을 보내는 서버측에서 `Mimetype` 헤더를 `text/xml`로 지정해 주면 xml형식으로 보여준다

가령 php에서는 소스 첫부분에

{% highlight php %}
<?php
header("Content-Type:text/xml");
{% endhighlight %}

을 선언해 준후 xml 컨텐츠를 내보내면 아래와 같이 정상적으로 보이게 된다

![xml 파싱 이미지](/assets/uploads/xml-after.png "xml entities")

다른 mimetype에 대해 알고 싶으면 아래 링크 참고

<http://en.wikipedia.org/wiki/Mimetype>