---
author: Kent Chiu
published: true
layout: post
title: "Upgrade PHP to 5.3"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - centos
---






```
    wget http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-3.noarch.rpm
    wget http://rpms.famillecollet.com/enterprise/remi-release-5.rpm
    rpm -Uvh remi-release-5*.rpm epel-release-5*.rpm
    yum --enablerepo=remi update php

```


```
php -v
 
PHP 5.3.0 (cli) (built: Jul 19 2009 17:55:08)
Copyright (c) 1997-2009 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2009 Zend Technologies

```

if you got dependency issue, you should also update mysql as same time
use this command



```
    yum --enablerepo=remi update php mysql

```

