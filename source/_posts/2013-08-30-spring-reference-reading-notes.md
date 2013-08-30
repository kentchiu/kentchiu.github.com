---
published: false
author: Kent Chiu
layout: post
title: "Spring Reference Reading Notes"
date: 2013-08-30 11:37
comments: true
sharing: true
footer: true
categories: 
---


使用setter 或 constructor inject的方式會比較容易測試，因為不需要其他service locator (IOC container本身就是一種 service locator)

設定檔的路徑，應該要用絕對路徑，而非相對路徑