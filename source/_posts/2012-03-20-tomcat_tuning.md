---
author: Kent Chiu
published: true
layout: post
title: "Tomcat Tuning"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - jvm
  - java
  - performace
  - memory
  - profile
---




Tomcat預設的記憶體使用量太小，調整方式如下

修改bin\\catalina.sh或bin\\catalina.bat

```
JAVA_OPTS="-Djava.awt.headless=true -Dfile.encoding=UTF-8
-server -Xms1024m -Xmx1024m
-XX:NewSize=512m -XX:MaxNewSize=512m -XX:PermSize=512m
-XX:MaxPermSize=512m -XX:+DisableExplicitGC"
```





