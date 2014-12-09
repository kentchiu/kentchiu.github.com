---
author: Kent Chiu
published: true
layout: post
title: "Enaled SSH"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ubuntu
  - linux
---



### install openssh server


```
sudo apt-get install openssh-server

```

### check installation


```
ps aux | grep ssh

```

ps should output more then 2 lines information.

ps - report a snapshot of the current processes

### config


```
sudo vim /etc/ssh/sshd_config

```

if you need to disable root login.

change


```
PermitRootLogin Yes 

```

to


```
PermitRootLogin no

```

if you want to change default listener port 22


```
# What ports, IPs and protocols we listen for 
Port 22 

```

### restart


```
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start

```

Please remember to configure allow hosts and deny hosts in production
environment.

### resource

-   [SSH
    setup](http://blog.udn.com/nigerchen/2262865 "http://blog.udn.com/nigerchen/2262865")


