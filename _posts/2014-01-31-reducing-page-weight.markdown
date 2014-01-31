---
layout: post
title:  "가벼운 웹사이트 만들기"
categories: programming 
date:   2014-01-31 00:49:28
tags:   
---
sitepoint에 [가벼운 웹사이트 만들기 4부작](http://www.sitepoint.com/series/reducing-page-weight/) 글이 올라와서 이에 대해 간략히 정리하고자 한다.

자세한 내용을 원하는 분들은 아래 주소의 글을 읽어보는 것을 추천한다.

1. [빠르고 쉬운 10가지 방법](http://www.sitepoint.com/ten-quick-fixes-reduce-page-weight/)
2. [조금 어려운 10가지 방법](http://www.sitepoint.com/10-tougher-tasks-reduce-page-weight/)
3. [웹페이지 성능 측정 도구 10가지](http://www.sitepoint.com/10-best-webpage-weight-analysis-tools/)
4. [사이트가 비대해 지는것을 막는 10가지 방법](http://www.sitepoint.com/10-drastic-ways-avoid-website-obesity/)

---

### 빠르고 쉬운 10가지 방법
#### **1. gzip 압축 활성화**
간단한 서버 설정으로 컨텐츠를 압축할수 있다.

#### **2. 브라우저 캐싱 활용**
expire header, last-modified, etags 헤더등을 활용할수 있다.

아파치 .htaccess를 이용해서 jpg같은 이미지 파일에 1달 캐시를 적용하는 방법은 아래와 같다.

    <IfModule mod_expires.c>
    ExpiresActive On
    
    <FilesMatch "\.(jpg|jpeg|png|gif|svg)$">
    ExpiresDefault "access plus 1 month"
    </FilesMatch>
    
    </IfModule>

(역주 : http 헤더를 이용해서 캐싱하는 방법에 대해서는 추가로 포스팅을 할 예정이다)

#### **3. CDN 사용**
브라우저는 하나의 도메인당 동시에 4~8개의 커넥션 제한을 두기 때문에 CDN을 분리하는것은 동시에 더 많은 커넥션을 맺을수 있다는 것을 의미한다. 또한 서드파티 라이브러리(jquery등)나 font 파일의 경우 외부에서 받은 리소스를 그대로 활용할수도 있고, 이미지에 특화된 CDN호스팅을 사용할수도 있다.

#### **4. 사용하지 않는 assets 삭제**
더이상 사용하지 않는 위젯이 생긴다면 그와 관련된 javascript코드와 css코드는 삭제하는 것이 좋다. 그 코드가 하나의 파일로 분리되어 있었다면 간단한 작업이 될테지만 그렇지 않다면 크롬 개발자 도구의 Audit 툴이나 [JSLint], [Dust-Me Selectors], [CSS Usage], [unused-css.com] 혹은 [grunt-uncss] 같은 빌드툴이 필요수도 있다.

#### **5. css 병합과 최소화(minify)**
하나의 css파일만 있는것이 가장 이상적이다. 유지보수를 위해 여러개의 css파일로 개발하더라도 production 서버에 올릴때에는 공백 제거등의 최소화 작업을 하는것이 좋다.

[Sass]나 [LESS], [Stylus] 같은 CSS Pre-processor 들을 활용할수도 있고 [Grunt.js] 나 [Gulp], [Koala]\(GUI제공) 같은 빌드툴을 이용할수도 있다.

이런 툴들을 사용하는대신 수동으로 해결하는 방법도 있다.

	copy file1.css+file2.css file.css	# Windows
    cat file1.css file2.css > file.css	# Mac/Linux

위 명령어를 이용해 파일들을 하나로 합친다음 최소화 작업을 위해 [cssminifier.com], [CSS Compressor & Minifier] 혹은 [CSS Compressor] 같은 온라인 minifier 툴을 이용하면 된다.

최종 css를 페이지에 넣을때에는 이후에 뿌려지는 요소들을 브라우저가 어떻게 그려야할지 미리 알수있게 &lt;head&gt; 태그 내에 두어서, 이미 그린 요소들을 다시 그리는 일이 없도록 하자.

#### **6. javascript 병합과 최소화(minify)**

jquery같은 라이브러리들은 현실적으로 별도의 파일로 두게 되겠지만 직접 작성하는 javascript 코드들은 production 상에서 하나의 파일로 최소화 시키도록 해야한다. 다시한번 말하지만, 이를 도와주는 여러가지 빌드툴이 있다. [YUI Compressor], [Closure Compiler] 그리고 개인적으로 가장 좋아하는 [The JavaScript CompressorRater] 같은 경우에는 여러 압축 엔진을 사용해서 그중에 가장 최적인것을 선택할수도 있게 한다.

만약 올바르지 않은 코드가 있다면 압축이 실패할 것이기 때문에 자바스크립트 압축은 좀더 신중하게 해야한다. 

js파일을 페이지에 포함시키기 가장 좋은 위치는 닫는 body 태그 바로 앞이다. 이렇게 하면 자바스크립트가 다운되고 실행되기 전까지 다른 컨텐츠들의 로딩을 막지 않아서 가독성이 좋아진다.

#### **7. 올바른 이미지 포멧 사용**
올바르지 않은 이미지 포멧 사용은 사이트를 무겁게 만든다. 일반적으로

1. 사진은 JPG
2. 나머지는 모두 PNG를 사용한다.

드물긴 하지만, 크기가 작으면서 색이 많이 사용되지 않은 이미지인 경우 GIF가 더 좋을수도 있다. 어떤 이미지는 벡터 이미지가 더 좋을수도 있는데 이에 대해서는 추후 설명하기로 한다.

이미지 변환을 위한 프로그램이 필요할수도 있겠지만 [XnView] 같은 무료 프로그램도 있다. 아래와 같이 설정하는것을 기억하자

- jpg는 0(화질 최하) 부터 100(화질 최상) 까지 화질 손상 정도를 정할수 있다. 대부분의 이미지들은 30~70 사이가 무난하겠지만, 실제 실험을 통해 허용 가능한 최저 화질을 얻어내는 것이 좋다
- png는 256색과 24-bit색이 가능하다. 투명효과가 필요 없고 색생의 제한을 둬도 된다면 256색으로 압축하는것이 좋다.


#### **8. 이미지 리사이징**

300메가 픽셀 짜리 보급형 스마트폰으로 찍은 사진들도 웹페이지에서 보여주기에는 크기가 너무 크다. 예를 들어, 좌우 800픽셀 짜리 페이지라면 레티나 디스플레이를 고려하더라도 1600픽셀 이미지면 충분하다. 길이가 1/2으로 줄면 크기는 1/4로 줄어들게 되므로 필요한 만큼 리사이징을 하게되면 그 효과는 매우 크다.

#### **9.  이미지를 더 압축하기**
포멧도 바꾸고 리사이징까지 했지만 그래픽 분석/최적화 툴을 이용하면 이미지 용량을 좀더 줄일수도 있다. [OptiPNG], [PNGOUT], [jpegtran], [jpegoptim] 등이 있고 클라우드에서 작업할수 있는 온라인 서비스 [Smush.it]도 있다.

#### **10. 필요없는 폰트 제거**
커스텀 폰트는 디자인 요소로서는 매우 좋지만 수백kb의 용량을 차지한다. 단지 한줄의 제목을 위해 200kb의 폰트가 필요한지 신중히 생각해보자.

---

### 조금 어려운 10가지 방법

#### **1. SNS 버튼 제거**
페이스북+트위터+구글플러스+링크드인의 SNS의 공유 버튼은 [580kb나 차지][580kb of content]할 뿐만 아니라 페이지의 내용이 모두 보여지기 전에 로드된다. SNS 공유를 위해 이런 것들이 꼭 필요한 것은 아니다. 몇줄의 html만 추가하면 [가벼운 SNS 공유 버튼][add fat-free social buttons to your pages]을 만들수 있다.

#### **2. 모든 서드파티 위젯들 검사**
SNS버튼들만 문제가 있는것은 아니다. 서드파티 위젯들의 사용은 추가적인 비용을 동반한다. 어쩔수 없이 써야한다면 좀더 좋은것을 골라서 사용하자

#### **3. Lazyloading 혹은 on-demand 컨텐츠 사용**
페이지가 스크롤 됨에따라 가시영역의 이미지를 로딩하는 방법을 고려해 볼수 있다. 이 방법은 검색엔진 최적화(SEO)에 부정적인 영향을 미칠수 있기때문에 별로 좋아하는 방법은 아니지만 사진 갤러리 같은 형태의 어플리케이션에서는 유용하게 사용될수 있다.

#### **4. 이미지를 CSS3 효과로 대체**
그라데이션이나 모서리가 둥근 사각형, 혹은 그림자 효과 같은 기능들은 css3에 존재하는 기능들이다. IE8과 그 이하 버전에서는 제대로 동작하지 않지만 그런 오래된 브라우저들은 점차 사라지고 있다. 하지만 잊지 말아야 할점은 그래디언트나 그림자 같은 기능들은 그리는 시점에 리소스를 많이 쓰기 때문에 무조건 이미지를 없애버리기 전에 성능 테스트를 해볼 필요가 있다.

#### **5. 자바스크립트를 CSS3 효과로 대체**
`$("#x").fade()`나 `$("#y").slideDown()` 같은 함수들은 몇년 전까지는 필요했으나 이제는 CSS3의 [animations], [transitions] and [transformations] 들로 대체할수 있다.

#### **6. Scalable Vector Graphics (SVGs) 사용**
SVGs 는 XML 내에 점, 선, 형태(shape)를 벡터로  정의한다. 사진이나 복잡한 이미지는 jpg나 png포멧이 더 좋지만, 로고, 다이어그램, 차트 등에는 SVGs가 적합할수도 있다. 뿐만 아니라 SVGs는 css나 javascript를 통해서 조작할수도 있다.

비트맵 이미지를 SVGs로 만들어 주는 툴들도 존재하지만, 처음 배울때 Illustrator나 [Inkscape], [SVG edit]로 시작한다면 좋은 결과를 만들어 낼수 있을것이다. 

#### **7. 이미지를 아이콘 폰트로 대체**
사이트 전체에 걸쳐 작은 아이콘들을 많이 사용하고 있다면 [아이콘 폰트][icon font]를 사용하는것도 고려할만 하다.하나의 폰트 파일 안에는 색상과 크기 설정이 가능한 수백개의 벡터 이미지들이 포함된다. 자신만의 최적화된 폰트를 써도 되고 [Fontello]나 [IcoMoon], [Flaticon] 에서 필요한 것들만 골라서 패킹할수도 있다.

#### **8. 이미지 스프라이트 사용**
비트맵 이미지의 사용은 CSS3나 SVG, 아이콘 폰트등을 사용할수 없을때 선택하는 최후의 수단이 되어야 한다. 자주 사용되는 이미지들을 하나로 묶어서 css를 사용해 하나씩 사용하도록 하자. 이렇게 하면 몇가지 장점들이 있다.

1. sprite된 이미지들을 하나의 http 요청으로 받아올수 있다.
2. 일반적으로 하나로 합친 이미지는 분리된 파일들의 총 용량보다 작다.
3. 하나의 이미지만 로드 되면, 모든 이미지들이 보여진다.

이런 작업을 도와주는 툴로는 [Sprite-Cow], [Instant Sprite]같은 것들이 있다.

#### **9. data URI 사용**
인코드된 텍스트나 바이너리 데이터들을 마치 리소스인 것처럼 쓸수 있다. 이렇게 해서 비트맵이나 SVGs 들을 html나 javascript, css 에서 바로 사용할수 있다.

{% highlight css %}
.bullet {
	background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQAQMAAAAlPW0iAAAABlBMVEUAAAD///+l2Z/dAAAAM0lEQVR4nGP4/5/h/1+G/58ZDrAz3D/McH8yw83NDDeNGe4Ug9C9zwz3gVLMDA/A6P9/AFGGFyjOXZtQAAAAAElFTkSuQmCC");
}
{% endhighlight %}

이 방식을 자동화 하지 않는다면 유지보수 하기는 힘들겠지만, http 요청을 회수를 줄일수 있다. 따라서 빈번히 사용되지만 자주 바뀌지는 않는 리소스에 이 방법을 적용하는것이 좋다. [DataURL.net], [data: URI Generator] 같은 곳에서 이미지를 data url로 변환할수 있다.

#### **10. 웹사이트 측정 툴 사용**
성능을 개선한다고 한들 모니터링을 하지 않는다면 사이트가 용량이 얼마나 줄었고 속도가 얼마나 빨라졌는지 모른다. [Google Page Speed Insights] 같은 툴들을 사용해보자. 


[JSLint]: http://www.jslint.com/
[Dust-Me Selectors]: https://addons.mozilla.org/en-US/firefox/addon/dust-me-selectors/
[CSS Usage]: https://addons.mozilla.org/ko/firefox/addon/css-usage/
[unused-css.com]: http://unused-css.com/
[grunt-uncss]: https://github.com/addyosmani/grunt-uncss
[Sass]: http://sass-lang.com/
[LESS]: http://lesscss.org/
[Stylus]: http://learnboost.github.io/stylus/
[Grunt.js]: http://gruntjs.com/
[Gulp]: http://gulpjs.com/
[Koala]: http://koala-app.com/
[cssminifier.com]: http://cssminifier.com/
[CSS Compressor & Minifier]: http://www.minifycss.com/css-compressor/
[CSS Compressor]: http://www.cssdrive.com/index.php/main/csscompressor/
[YUI Compressor]: http://www.refresh-sf.com/yui/
[Closure Compiler]: http://closure-compiler.appspot.com/home
[The JavaScript CompressorRater]: http://compressorrater.thruhere.net/
[XnView]: http://www.xnview.com/
[OptiPNG]: http://optipng.sourceforge.net/
[PNGOUT]: http://advsys.net/ken/utils.htm
[jpegtran]: http://jpegclub.org/jpegtran/
[jpegoptim]: http://jpegoptim.sourceforge.net/
[Smush.it]: http://www.smushit.com/ysmush.it/
[580kb of content]: http://www.sitepoint.com/social-sharing-hidden-costs/
[add fat-free social buttons to your pages]: http://www.sitepoint.com/social-media-button-links/
[animations]: http://www.sitepoint.com/css3-animations-101/
[transitions]: http://www.sitepoint.com/css3-transitions-101/
[transformations]: http://www.sitepoint.com/css3-transformations-101/
[Inkscape]: http://www.inkscape.org/en/
[SVG edit]: http://svg-edit.googlecode.com/svn/branches/2.6/editor/svg-editor.html
[icon font]: http://www.sitepoint.com/webfont-icons/
[Fontello]: http://fontello.com/
[IcoMoon]: http://icomoon.io/
[Flaticon]: http://www.flaticon.com/
[Sprite-Cow]: http://www.spritecow.com/
[Instant Sprite]: http://instantsprite.com/
[DataURL.net]: http://dataurl.net/#about
[data: URI Generator]: http://dopiaza.org/tools/datauri/index.php
[Google Page Speed Insights]: http://developers.google.com/speed/pagespeed/insights/