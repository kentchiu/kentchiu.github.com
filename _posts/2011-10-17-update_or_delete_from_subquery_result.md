---
author: Kent Chiu
published: true
layout: post
title: "Update Or Delete with Subquery reslut"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - tips
  - mysql
---


if you try to delete a table and that table is using in subquery like
this


```
DELETE FROM table_a WHERE col_a IN (SELECT col_a FROM table_a) 

```

The syntax will cause a error says “You can't specify target table 'm'
for update in FROM clause”, error code is 1093. This is cause by MYSQL
DON'T support update table which also used in subquery.

The solution is using alias like this


```
DELETE FROM table_a WHERE col_a IN (SELECT col_a FROM (SELECT col_a FROM table_a) AS table_x)

```


