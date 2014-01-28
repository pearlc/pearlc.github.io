---
layout: post
title:  "mysql 쿼리의 hint 옵션"
categories: programming mysql
date:   2011-07-04 01:26:05
tags:   mysql, hint, 힌트
---

5.1 버전 이후의 MySQL 쿼리 실행시, 특정 index 를 사용하도록(혹은 사용하지 않도록) 옵션을 지정해서 명시할수가 있다.

이 옵션은 hint 라고 불리우는데, MySQL이 때때로 잘못된 index를 사용해서 쿼리 성능이 저하될 경우에 사용하면 유용하다.

(현재 쿼리를 MySQL이 어떤 index를 사용해서 실행할지는 EXPLAIN 명령으로 알아낼수 있다)
 
예를들어  theTable 이라는 테이블에 indexA와 indexB 라는 인덱스가 존재할때, 명시적으로 indexA 를 사용하게 하고 싶다면

{% highlight mysql %}
select * from theTable use index indexA
{% endhighlight %}
    
라고 쿼리를 날리면 된다

`use index` 대신 `force index`로 바꿔도 유사하게 동작한다

이와는 반대로 특정 인덱스를 사용하고 싶지 않다면 `ignore index` 옵션을 사용하면 된다

{% highlight mysql %}
select * from theTable ignore index indexA
{% endhighlight %}