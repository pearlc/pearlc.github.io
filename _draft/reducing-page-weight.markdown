---
layout: post
title:  "가벼운 웹사이트 만들기"
categories: programming 
date:   2014-01-31 00:49:28
tags:   
---

sitepoint에 [가벼운 웹사이트 만들기 4부작](http://www.sitepoint.com/series/reducing-page-weight/) 글이 올라와서 이에 대해 간략히 정리하고자 한다.

자세한 내용을 원하는 분들은 아래 주소로 직접 가면 될듯 하다.
1. [빠르고 쉬운 10가지 방법](http://www.sitepoint.com/ten-quick-fixes-reduce-page-weight/)
2. [조금 어려운 10가지 방법](http://www.sitepoint.com/10-tougher-tasks-reduce-page-weight/)
3. [웹페이지 성능 측정 도구 10가지](http://www.sitepoint.com/10-best-webpage-weight-analysis-tools/)
4. [사이트가 비대해 지는것을 막는 10가지 방법](http://www.sitepoint.com/10-drastic-ways-avoid-website-obesity/)



### 빠르고 쉬운 10가지 방법
1. gzip 압축 활성화

2. 브라우저 캐싱 활용
expire header, last-modified, etags 를 활용.
아파치 .htaccess를 이용해서 jpg같은 이미지 파일에 1달 캐시를 적용하는 방법은 아래와 같다 
{% highlight %}
<IfModule mod_expires.c>
ExpiresActive On
 
<FilesMatch "\.(jpg|jpeg|png|gif|svg)$">
ExpiresDefault "access plus 1 month"
</FilesMatch>
 
</IfModule>
{% endhighlight %}

(역주 : 이 캐싱 방법에 대해서는 추가로 포스팅을 할 예정이다)


3. 