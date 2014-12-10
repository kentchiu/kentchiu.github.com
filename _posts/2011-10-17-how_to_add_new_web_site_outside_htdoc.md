---
author: Kent Chiu
published: true
layout: post
title: "在htdoc目錄外新增網站"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - apache
---





```
        Alias /dokuwiki "D:/docs/wiki/dokuwiki"
        <Directory "D:/docs/wiki/dokuwiki">
            AllowOverride None
            Order Deny,Allow 
        </Directory>

```

如果是這樣，可能會有問題


```
AllowOverride None
Order Deny,Allow
Deny from all 

```