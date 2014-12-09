---
author: Kent Chiu
published: true
layout: post
title: "Programming Tips"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - programming
---




-   儘量採用TDD(Test Driven Development)方式開發程式
-   避免使用static method跟singleton pattern，因為這對測試不友善
-   如果使用singleton
    pattern，記得採用[n](http://wiki.kent-chiu.com/doku.php?id=prog:n "prog:n")，這會比較好測
-   每個method應該儘可能的小
-   Class name, method name, varible
    name應能直覺得表示其責任，不要害怕採用長一點的程式名稱
-   變數作用域(scope)應該儘可能的小

