---
author: Kent Chiu
published: true
layout: post
title: "在ibus下使用無蝦米"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ubuntu
  - linux
---




ibus取得表格檔
--------------

-   [liu-ibus](http://wiki.kent-chiu.com/lib/exe/fetch.php?media=ubuntu:liu-ibus.zip "ubuntu:liu-ibus.zip")
    表格檔部份源自李果正的[noseeing
    table](http://edt1023.sayya.org/gcin/noseeing-12.tar.gz "http://edt1023.sayya.org/gcin/noseeing-12.tar.gz")
    ，ICON來自GCIN

### 轉換成ibus用的表格檔格式

```
ibus-table-createdb -s noseeing*scimtxt
```

### copy 表格檔跟icons到ibus目錄

```
sudo cp noseeing.db /usr/share/ibus-table/tables/
sudo cp noseeing.png /usr/sha*e/*bus/icons/noseeing.png
```

登出/登入 測試

### 設定

參考這裡[http://boshiamy.com/member\_download.php](http://boshiamy.com/member_download.php "http://boshiamy.com/member_download.php")


