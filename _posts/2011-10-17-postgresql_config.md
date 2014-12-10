---
author: Kent Chiu
published: true
layout: post
title: "設定PostgrSQL的連線配置"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - postgresql
---



設定可以連線的IP是在 data\\postgresql.conf



```
    listen_addresses = '*'        # what IP address(es) to listen on;

```

設定連線的認證方式是在 data\\pg\_hba.conf



```
    # TYPE  DATABASE        USER            CIDR-ADDRESS            METHOD
    # IPv4 local connections:
    #host    all             all             127.0.0.1/32            trust
    # IPv6 local connections 允許來自同網域的1~128網段的使用者連線，不需密碼
    #host    all             all             ::1/128                trust 
    # password authorize     允許來自同網域的1~128網段的任何使用者連線，需密碼
    host      all            all             ::1/128                md5

```

USER設為ALL表示，資料庫裡所有的user都能連進資料庫，沒有的user就不能連

如果忘記db密碼，可以設成


```
host    all             all             ::1/128                trust 

```

允許同網段的ip連入，之後可以透過postgres這個預設的user登入之後，修改postgres
user的密碼，再改回用密碼認證的方式。

