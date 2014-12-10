---
author: Kent Chiu
published: true
layout: post
title: "Deploy Maven Artifacts to Nexus Server"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - maven
---




maven的deploy指令可以直接將artifact deploy到nexus server

### pom.xml的設定

<http://host:8080/nexus>是可以連到nexus server的url
`<id>nexus</id>`是辨識用的repository id,必須跟下面的settings.xml一致


```
<distributionManagement>
    <!-- use the following if you're not using a snapshot version. -->
    <repository>
        <id>nexus</id>
        <name>Internal Releases</name>
        <url>http://host:8080/nexus/content/repositories/releases</url>
    </repository>
    <!-- use the following if you ARE using a snapshot version. -->
    <snapshotRepository>
        <id>nexus</id>
        <name>Internal Releases</name>
        <url>http://host:8080/nexus/content/repositories/snapshots</url>
    </snapshotRepository>
</distributionManagement>

```

### user\_home/.m2/settings.xml設定

`<id>nexus</id>`是辨識用的repository id,必須跟上面的pom.xml中的一致


```
<servers>
    <server>
        <id>nexus</id>
        <username>username</username>
        <password>password</password>
    </server>
    <server>
        <id>thirdparty</id>
        <username>username</username>
        <password>password</password>
    </server>
</servers>

```

### deploy

執行以下的指令即可進行deploy


```
mvn deploy

```

如果執行時遇到 401 的error code，表示帳號或密碼有誤

