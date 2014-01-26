---
layout: post
title:  "워드프레스(wordpress)에서 지킬(jekyll)로 갈아타다"
date:   2014-01-26 23:00:00
categories: life
tags:   wordpress, jekyll, migrate, jinolog
---

새해가 한달 가까이 지났지만 그래도 새해가 왔으니 뭔가 변화를 줘야겠다는 생각이 들어서 주기적으로 프로그래밍 글을 써보기로 마음을 먹었다.

물론 나에게 블로그는 있었지만 근 1년간 방치되서 다 쓰러져 가고 있던 터라, 일단 이곳을 리뉴얼 하기로 마음을 먹고 요즘 트렌드에 따라 github + jekyll을 이용해서 새 단장을 했다..

jekyll에서 정식으로 제공하는 migration 툴을 사용한 것은 아니고, 마크다운 문법도 익힐겸 수동으로 컨텐츠를 재작성 했다. (사실 migration 툴 설치중에 발생한 오류 해결하기 귀찮아서..)

한가지 아쉬운 점은 기존 글들이 가지고 있던 permanent url 들의 url이 모두 사라지게 되었다는점이다. redirect 방법과 검색엔진에게 컨텐츠 url이 이동되었다는 것을 알려주는 방법을 고민해봤는데, 301 응답을 쏘는것도 github에서 제공하지 못하고, 워드프레스의 entrypoint 였던 index.php코드를 작동시키는 것도 불가능 해서 결국 모두 사라지게 되었다.
아쉽지만 혹시 나중에라도 방법을 알게 된다면 포스팅 해봐야 겠다.

테마도 지금은 매우 심플한데, 검색기능이나 gnb, seo같은 기능들을 좀 추가해 봐야겠다.

[jekyll]:    http://jekyllrb.com
