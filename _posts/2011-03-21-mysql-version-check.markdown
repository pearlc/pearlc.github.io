---
layout: post
title:  "MySQL의 버전 확인"
categories: programming mysql
date:   2011-03-21 00:44:45
tags:   mysql, version
---

MySQL의 버전을 확인하기 위해서는 커맨드 창에서 아래와 같이 입력하면 된다

    # mysql -V
    
또는

    # mysql -version
    
하지만 이는 엄밀히 말하면 mysql 클라이언트의 버전을 체크하는 방법인데, (경우에 따라 서버의 버전이 나오기는 한다) 서버의 버전을 체크하기 위해서는 sql서버에 접속한후 아래의 쿼리를 날리면 된다

    mysql> SELECT VERSION();
    
또는

    mysql> SHOW VARIABLES LIKE '%VERSION%'