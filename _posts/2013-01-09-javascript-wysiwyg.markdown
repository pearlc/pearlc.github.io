---
layout: post
title:  "자바스크립트 위지윅(WYSIWYG) 에디터"
categories: programming javascript
date:   2013-01-09 01:07:58
tags: javascript-wysiwyg-editor, ckeditor, tinymce, redactor, 자바스크립트에디터
---

요즘 쓸만한 자바스크립트 위지윅 에디터를 찾고 있는데, 여기저기 알아본 결과 2013년 1월 현재 쓸만한 에디터는 대략 아래의 3개로 압축되는것 같다.

1. [redactor] - 유료
2. [tinyMCE] - 오픈소스 LGPL라이센스
3. [ckEditor] - 오픈소스 GPL라이센스

이중 1번 redactor 는 유료라서 패스 하고-_ -; 2번 tinyMCE는 오래되고 기능도 많아서 좋긴하지만 역시 3번 ckEditor가 정답인듯.

원래 fckEditor가 전신인 ckEditor는, 2012년 11월에 4.0을 업데이트를 하면서 확실하게 앞서나가는 모습이다. 개발팀 스스로도 자신감이 붙었는지 4.0 업데이트때 블로그 포스팅 슬로건도 [The Future of Web Editing][ckeditor-blog-post] 이었다. 

ckEditor의 장점으로는 크로스브라우징지원, 다양한 플러그인, 자세한 설명 문서, 손쉬운 커스터마이징, 강력한 모듈화 등등이 있는데, 플러그인을 대충 살펴보니 웬만한건 다 있는듯 싶고, 만약 없다면 플러그인 만들기도 편해서 그다지 큰 문제가 되지는 않을것 같다.

개발버전의 코드도 github에 별도로 공개되어 있어서 자바스크립트 공부하기도 좋을것 같기도 한데 자바스크립트 코드는 아무리 봐도 모르겠다는게 함정..

- 설명문서 : <http://docs.ckeditor.com/>
- 개발버전소스코드 : <https://github.com/ckeditor/ckeditor-dev>

호기심에 구글트렌드로 쿼리량을 검색해보니 줄곧 tinyMCE가 앞서다가 작년말부터 ckeditor가 앞서는 모양. 앞으로는 이런 추세가 이어질것 같다.

<script type="text/javascript" src="//www.google.com/trends/embed.js?hl=ko&q=tinymce,+ckeditor&date=1/2011+25m&cmpt=q&content=1&cid=TIMESERIES_GRAPH_0&export=5&w=628&h=330"></script>

[redactor]: http://imperavi.com/redactor/
[tinyMCE]: http://www.tinymce.com/
[ckEditor]: http://ckeditor.com/
[ckeditor-blog-post]: http://ckeditor.com/blog/CKEditor-4-Launched-Inline-Editing-New-Skin-and-More