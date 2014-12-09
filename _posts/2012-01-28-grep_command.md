---
author: Kent Chiu
published: true
layout: post
title: "grep command"
date: 2012-01-28
comments: true
external-url:
sharing: true
footer: true
categories:
  - command
  - linux
---



Syntax: grep [options] pattern [files]

##### 找某個檔案中是否有存在特定文字


	# grep foobar ~/myfile.txt

##### 計算特定文字出現的次數

	# grep -c foobar ~/myfile.txt


##### 找有的目錄下的當案是否有存在特定文字

	# grep -r foobar ~/
	# grep -r foobar *




##### 使用Regular Expression

	# grep "^foobar" ~/myfile.txt

可以用”\^\$”來找出空白行


