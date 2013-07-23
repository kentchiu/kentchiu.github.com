---
published: false
author: Kent Chiu
layout: post
title: "URL encoding"
date: 2013-06-25 11:01
comments: true
sharing: true
footer: true
categories: 
- encoding
- rest
---


`https://bob:bobby@www.lunatech.com:8080/file;p=1?q=2#third`

- Scheme           : https
- User             : bob
- Password         :bobby
- Host address     : www.lunatech.com
- Port             : 8080
- Path             : /file
- Path parameters  : p=1
- Query parameters : q=2
- Fragment         : third

###### Path parameters
Path parameters 又叫 Matrix Parameters, 每個 *path segment* 可以有自已的 Matrix Parameters，這在 Restful style 的設計上有時會很有用。

###### Fragment

Fragment 是用來指出整份 URL resrouce 的某一特定部份，在網頁設計上是用來做定位用的錨點 (anchor)

#### 保留字處理

path 跟 query string 對保留字的處理方式不一樣，所以，在做編解碼時，要分開處理

ex:
空白字元在 path 會被編成 %20 , '+' 會被編成 '_' ,但在 query string 空白字元會被編成 '+' or '%20', '+' 會被編成 '%2B'

所以，如果有一個`blue+light blue`同時放在 path 跟 query string，那結果會是這樣

 	http://example.com/blue+light%20blue?blue%2Blight+blue

#### 編碼、中文與 Unicode

RFC 1738 並沒有規定要用什麼樣的編碼，所以，一般會在 HTTP header 指定 encoding 或採用 HTML page encoding


## Resource
  - <http://blog.lunatech.com/2009/02/03/what-every-web-developer-must-know-about-url-encoding> - What every web developer must know about URL encoding





