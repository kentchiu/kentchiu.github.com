---
published: true
author: Kent Chiu
layout: post
title: "Java 8 好用工具箱"
date: 2014-03-14 12:43
comments: true
sharing: true
footer: true
tags: 
- java8 
---


java 8 多了不少好用的工具類class，例如: 字串處理，null處理…之前透常是透過[google guava](https://code.google.com/p/guava-libraries/)，或[apache common lang](http://commons.apache.org/proper/commons-lang/)來處理，現在java 8 就有內建了。

- [Optional](http://download.java.net/jdk8/docs/api/java/util/Optional.html) 更安全、方便處理NULL的物件
- Objects
- StringJoin


#### Objects 
- requireNonNull(T obj) 如果傳入的obj為空，會丟出` NullPointerException`異常