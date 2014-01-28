---
layout: post
title:  "HHVM(hiphop virtual machine) for PHP"
categories: programming php
date:   2014-01-27 09:00:00
tags: php, hhvm, hiphop php, zend, 힙합, 힙합php, facebook, compiled, interpreted, 컴파일드, 인터프리티드, 컴파일드언어, 인터프리티드언어, 컴파일러, 인터프리터, 젠드엔진
---

hhvm은 facebook이 만든 php 언어 변환기이자 virtual machine 이다.

hhvm이 어떻게 동작하는지를 알기 위해서는 먼저 컴파일드 언어와 인터프리티드 언어의 차이점에 대해서 알아야 한다.

### 컴파일드 언어 vs 인터프리티드 언어

#### 컴파일드 언어 (compiled language)
컴파일드 언어는 기계어로 컴파일 되는 언어이다. 프로그래머가 작성한 코드는 "컴파일러" 라는 프로그램에 의해 "컴파일" 이라는 과정을 거치게 된다. 이렇게 컴파일된 언어는 컴퓨터가 알아볼기 쉽게 기계어로 변환이 되서 실행이 된다.

#### 인터프리티드 언어 (interpreted language)
인터프리티드 언어는 위에 언급한 "컴파일" 과정이 필요없는 언어이다. 대신 인터프리터(interpreter)라는 프로그램이 프로그래머가 작성한 코드 한줄한줄이 실행 될때마다 그에 대응되는 native 코드로 변환을 한후 실행을 시킨다.

컴파일드 언어는 컴파일 중에 최적화 과정을 거치기 때문에 실행되는 속도가 매우 빠르다는 장점이 있지만 반드시 컴파일 과정을 거쳐야 하므로 프로그래머 입장에서 번거로울수도 있다. (프로그램 규모에 따라서 컴파일 시간이 매우 길어지기도 한다) 인터프리티드 언어는 컴파일 과정이 필요 없지만 실행 속도가 상대적으로 느리다는 단점이 있다.

컴파일드 언어에는 C, Pascal, GO 등이 있고 인터프리티드 언어는 PHP, Javascript, Python, Ruby등이 있다.

Java 와 같이 두가지 특징을 모두 가지고 있는 언어도 있는데, java코드는 컴파일 과정에서 JVM(Java Virtual Machine) 에 최적화 되어있는 bytecode 로 변환되고 실행되는 시점에 JVM에 의해 native 코드로 다시 변환되서 실행된다.

### PHP 구동 환경

앞서 말했다 시피 php 언어는 인터프리티드 언어이기 때문에 실행이 되기 위해서는 Zend Engine 이라는 php interpreter가 반드시 설치되어 있어야 한다. 이 Zend Engine는 cgi인터페이스를 통해 웹서버와 통신하는데, 요청이 들어오면 php코드를 opcode로 변환(interpreting)한후 실행 결과를 반환한다.

![php 구동 환경]

### HipHop for PHP
한편 php를 주력 언어로 사용하고 있는 facebook은 facebook.com 웹사이트의 실행속도를 빠르게 하기 위해 php를 c++로 변환하는 번역기인 hiphop for php를 개발하고 2010년 발표와 함께 오픈소스로 공개[한글 번역][http://enzine.tistory.com/580]한다. php인터프리터인 Zend를 사용하지 않고 hiphop 을 이용해 c++코드로 변환한후 이것을 컴파일 해서 성능을 개선시키기 위함이었다. 발표에 따르면 이로인해 페이스북의 cpu 사용량이 50




[php 구동 환경]:    /assets/uploads/php-env.png

