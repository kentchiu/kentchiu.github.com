---
author: Kent Chiu
published: true
layout: post
title: "最佳實踐"
date: 2012-01-08
comments: true
external-url:
sharing: true
footer: true
categories:
  - sql
  - database
---



SQL DML
=======

-   'in'的效能比'or'好
-   避免使用負向的查詢， NOT、!=、\<\>、!\<、!\>、NOT EXISTS、NOT
    IN、NOT LIKE等
-   避免將'%'放在查詢的條件前面 ex: '%foo' (full table scan)
-   避免select (\*)
-   减少COUNT(\*)
-   短的sql比長的sql好


