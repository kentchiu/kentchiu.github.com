---
author: Kent Chiu
published: true
layout: post
title: "LAMP install on CentOS5"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - centos
  - php
  - apache
  - linux
  - mysql
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




OS installation
---------------

### VMWare

-   掛載iso檔
-   網路卡設成NAT
-   記憶體可調小一點300M應該就夠了
-   不用特別去裝VMTools

### CentOS 5

-   安裝成text mode的server就可(不用gui)
-   照[這篇](http://smartraining.cn/centos/server_installation "http://smartraining.cn/centos/server_installation")的方式去裝就好了
-   網路設定如以改用dhcp
-   Firewall disabled

#### 新建一個一般usre


```
adduser username
passwd username 

```

#### 網路設定

-   裝完後，如果網路沒有active，可以到/etc/sysconfig/network-scripts/ifcfg-eth0
    修改網路設定

指定IP



```
    DEVICE=eth0                          #網路卡名稱
    BOOTPROTO=static                     #BOOTP協定
    BROADCAST=192.168.202.255            #廣播 IP Address
    HWADDR=00:15:C5:E5:99:B1             #MAC Address
    IPADDR=192.168.202.100               #IP Address
    NETMASK=255.255.254.0                #Netmask (遮罩)
    NETWORK=192.168.202.0                #網段
    ONBOOT=yes                           #開機自動啟動 

```

DHCP



```
    DEVICE=eth0
    BOOTPROTO=dhcp
    ONBOOT=yes

```

設定完重新啟動網卡


```
#/etc/rc.d/init.d/network restart

```

#### 安裝packages

-   記得透過[yum](http://smartraining.cn/centos/yum "http://smartraining.cn/centos/yum")裝以一下以下幾個packages，滿有用的


```
yum -y  install wget bzip2 unzip zip nmap tree lynx

```

Server installation
-------------------

預設的php是5.1.x的，有點太舊了，可以透過[這個連結](http://www.52crack.com/blog/?action=show&id=368 "http://www.52crack.com/blog/?action=show&id=368")的說明更新到5.2.x

### MySQL 5

MySQL install


```
yum -y install mysql-server

```

set MySQL as daemon


```
chkconfig mysqld on

```

check config


```
chkconfig --list mysqld
# 2 - 5 should be on
mysqld 0:off 1:off 2:on 3:on 4:on 5:on 6:off

```

startup MySQL server


```
/etc/rc.d/init.d/mysqld start

```

change password


```
mysqladmin -u root password 'Your Password'

```

### Apache 2

install Apache server


```
yum -y install httpd

```

### PHP 5

install PHP


```
yum -y install php php-mbstring php-mysql php-gd php-pear

```

setup php.ini


```
date.timezone = "Asia/Taipei"

```

### Apache Config

set Apache as daemon


```
chkconfig httpd on

```

check config


```
chkconfig --list httpd 
# 2 - 5 should be on
httpd  0:off 1:off 2:on 3:on 4:on 5:on 6:off

```

startup httpd server


```
/etc/rc.d/init.d/httpd start

```

verify installed
----------------

create new php file in document root


```
vi /var/www/html/index.php

```



```
    <?php 
    phpinfo();
    ?>

```

open by lynx borwser


```
lynx http://localhost

```

you should check
[this](http://wiki.kent-chiu.com/doku.php?id=ubuntu:lamp "ubuntu:lamp")
article to config LAMP server

如果無法從linux外存取web，可能會firewall的關係，在command
line下執行`setup`可以關掉firewall





