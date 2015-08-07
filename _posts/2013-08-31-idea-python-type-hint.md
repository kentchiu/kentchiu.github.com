---
published: true
author: Kent Chiu
layout: post
title: "IDEA Python type hint"
date: 2013-08-31 15:27
comments: true
sharing: true
footer: true
tags: 
- idea
- ide
- python
- type hint
---

像python這種需要在執行時期才會正確知道型別的語言，IDE為了完善 code complete 的功能，都會加上一些type hint (型別提示)的輔助功能，IDEA在python的型別提示主要是透過兩個方式
1. 文件註釋
2. method 參數及返回值的型別

## method 參數及返回值的型別 ##

	def is_image(link: str) -> bool:

在參數*link*後面加上型別提示，變成`link: str`及在返回值加上` -> bool`，IDEA便可以在開發時的code complete正確的列出侯選值

Resources
---------
- [IDEA document](http://www.jetbrains.com/pycharm/webhelp/type-hinting-in-pycharm.html) - type hint