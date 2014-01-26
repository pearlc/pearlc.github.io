---
layout: post
title:  "X-Forwarded-For(XFF) HTTP 헤더"
categories: network
date:   2011-07-05 00:52:07
tags:   xff, x-forwarded-for, header, proxy, 프록시, ip
---

X-Forwarded-For(XFF) HTTP 헤더 영역은 HTTP 프록시 서버나 로드 밸런서를 통과해 들어오는 클라이언트의 본래 IP주소를 알아내는데 사용되는 영역이다

Squid 프록시 서버에 의해 소개되었고 RFC 표준은 아니지만, 사실상의 표준(*de facto standard*)으로 인정받고 있다

XFF 필드를 보면 일반적으로 아래와 같이 되어 있는데

    X-Forwarded-For: client1, proxy1, proxy2

각각의 IP는 \[콤마+빈칸\] 으로 연결되어 있으며, 왼쪽에 있는 IP일 수록 받는 측에서부터 멀리 있는 지점이다

다시말하면 client1에서 전송 후 `proxy1` -> `proxy2` -> `proxy3` 를 거쳐서 현재 위치로 오게 된다

(proxy3는 Request의 Remote Address 로 나타남)