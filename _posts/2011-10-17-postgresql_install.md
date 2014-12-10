---
author: Kent Chiu
published: true
layout: post
title: "在Unbuntu安裝PostgreSQL"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - ubuntu
  - database
---



透過apt-get安裝


```
sudo apt-get install postgresql

```

因為pgsql預設一開始只有postgres能使用，所以，必需先切換成至postgres user


```
# 建立db
sudo -u postgres createdb mydb
# 建立user(role)
sudo -u postgres createuser --superuser $USER
# 執行 pg sql client
sudo -u postgres psql
# postgres# 表示已在psql command line模式下
# \password 改變user的密碼
postgres# \password

```

建立db跟user(非必要)


```
# 建立db
sudo -u postgres createdb mydb
# 建立user(role) 加--superuser會有db superuser的權限 $USER -> 可換成你想要的user name
sudo -u postgres createuser --superuser $USER

```

開放外部ip連入 postgresql
=========================


```
sudo vi /etc/postgresql/8.4/main/postgresql.conf
listen_addresses = '*' <-拿掉註解

```


```
sudo vi /etc/postgresql/8.4/main/pg_hba.conf
# Database administrative login by UNIX sockets
local   all         postgres                          ident
 
# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD
 
# "local" is for Unix domain socket connections only
local   all         all                               ident
# IPv4 local connections:
host    all         all        127.0.0.1/32               md5
#host    all         all        218.174.206.1/32           password
 
# IPv6 local connections:
host    all         all         ::1/128                 md5
 
# 加這行，讓所有外部ip可以透過password的方式連入
host    all         all        0.0.0.0/0                 password

```

改完restart server讓設定生效


```
/etc/init.d/postgresql-8.4 restart

```

這邊使用的postgresql是8.4版，如果版本不同，版號要適當調整

Resources
=========


