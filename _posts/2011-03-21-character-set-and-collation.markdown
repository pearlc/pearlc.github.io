---
layout: post
title:  "문자셋(Character set) 과 콜레이션(Collation)"
categories: programming mysql
date:   2011-03-21 00:52:13
tags:   mysql, character, set, collation, 캐릭터셋, 콜레이션
---

CHAR, VARCHAR, TEXT 등의 문자열 Datatype에는 문자셋(Character set)과 콜레이션(Collation) 속성이 있다.

문자셋(Character set)은 각 문자가 컴퓨터에 저장될 때 어떠한 '코드'로 저장될지에 대한 규칙의 집합을 의미하며 콜레이션(Collation)은 특정 문자 셋에 의해 데이터베이스에 저장된 값들을 비교 검색하거나 정렬 등의 작업을 위해 문자들을 서로 '비교' 할때 사용하는 규칙들의 집합을 의미한다.

같은 문자셋이라도 콜레이션에 따라 영어의 경우 대소문자의 구분 비교 여부, 일본어의 경우 히라가나와 카타카나의 구분 방법 등이 달라진다.

UTF8 문자셋을 사용하는 경우 `utf8-general-ci` 또는 `utf8-unicode-ci` 둘중 하나를 collation으로 지정하는 경우가 많은데 `utf8-general-ci`는 `utf8-unicode-ci` 를 사용해서 정렬할 때 보다 다소 정확도가 떨어지는 경향이 있으나 속도는 빠르다.

MySQL의 경우 `utf8-general-ci` 가 Default Collation이다.