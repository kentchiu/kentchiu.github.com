---
author: Kent Chiu
published: true
layout: post
title: "Liu in Linux (gcin)"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ubuntu
  - linux
---





Install gcin
------------

```
sudo apt-get install gcin
sudo im-switch -s gcin
```

logout /login

```
echo $XMODIFIERS
// output 
@im=gcin
```

install liu
-----------

```
mkdir temp
cd temp
wget http://edt1023.sayya.org/misc/noseeing-12.tar.gz
tar zvxf noseeing-6.tar.gz
mv noseeing.gtab ~/.gcin
```

logout/ login

noseeing-xx, current version is 12 (2010/3/17), but may be newer version
existing. you better check it.

setup
-----

check this
[http://boshiamy.com/member\_download.php](http://boshiamy.com/member_download.php "http://boshiamy.com/member_download.php")

Resources
=========

-   [table file](http://wiki.kent-chiu.com/lib/exe/fetch.php?media=ubuntu:noseeing-12.tar.gz "ubuntu:noseeing-12.tar.gz")
    - from
    [http://edt1023.sayya.org/misc/noseeing-12.tar.gz](http://edt1023.sayya.org/misc/noseeing-12.tar.gz "http://edt1023.sayya.org/misc/noseeing-12.tar.gz")
-   [http://boshiamy.com/member\_download.php](http://boshiamy.com/member_download.php "http://boshiamy.com/member_download.php")


