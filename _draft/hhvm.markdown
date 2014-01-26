---
layout: post
title:  "HHVM(hiphop virtual machine) for PHP"
categories: programming php
date:   2013-01-27 09:00:00
tags: php, hhvm, hiphop php, zend
---

hhvm은 facebook이 만든 php 언어 변환기이자 virtual machine 이다.

hhvm이 어떻게 동작하는지를 알기 위해서는 먼저 compiled language 와 interpreted language 의 차이점에 대해서 알아야 한다.

## 컴파일드 언어(compiled language) vs 인터프리티드 언어(interpreted language)

#### 컴파일드 언어(compiled language)
컴파일드 언어는 기계어로 컴파일 되는 언어이다. 프로그래머가 작성한 코드는 "컴파일러" 라는 프로그램에 의해 "컴파일" 이라는 과정을 거치게 된다. 이렇게 컴파일된 언어는 컴퓨터가 알아볼기 쉽게 기계어로 변환이 되서 실행이 된다.

#### 인터프리티드 언어 (interpreted language)
인터프리티드 언어는 위에 언급한 "컴파일" 과정이 필요없는 언어이다. 대신 인터프리터(interpreter)라는 프로그램이 프로그래머가 작성한 코드 한줄한줄이 실행 될때마다 그에 대응되는 native 코드로 변환을 한후 실행을 시킨다.

컴파일드 언어는 컴파일 중에 최적화 과정을 거치기 때문에 실행되는 속도가 매우 빠르다는 장점이 있지만 반드시 컴파일 과정을 거쳐야 하므로 프로그래머 입장에서 번거로울수도 있다. (프로그램 규모에 따라서 컴파일 시간이 매우 길어지기도 한다) 인터프리티드 언어는 컴파일 과정이 필요 없지만 실행 속도가 상대적으로 느리다는 단점이 있다.

컴파일드 언어에는 C, Pascal, GO 등이 있고 인터프리티드 언어는 PHP, Javascript, Python, Ruby등이 있다.

Java 와 같이 두가지 특징을 모두 가지고 있는 언어도 있는데, java코드는 컴파일 과정에서 JVM(Java Virtual Machine) 에 최적화 되어있는 bytecode 로 변환되고 실행되는 시점에 JVM에 의해 native 코드로 다시 변환되서 실행된다.

## PHP 동작 과정

앞서 말했다 시피 php 언어는 인터프리티드 언어이기 때문에 실행이 되기 위해서는 Zend Engine 이라는 php interpreter가 반드시 설치되어 있어야 한다.
이 Zend Engine이 웹서버와 (cgi 라고 하는 인터페이스를 통해) 통신 하면서 요청과 응답을 주고받게 된다.

## PHP 실행 속도를 높이기 위한 노력


