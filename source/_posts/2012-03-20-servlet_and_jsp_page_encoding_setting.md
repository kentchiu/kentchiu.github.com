---
author: Kent Chiu
published: true
layout: post
title: "Servlet And JSP encoding setting"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - jsp
  - java
---





Setting encoding filter and JSP page encoding in web.xml could fix
encoding issue.

servlet filter.

JSP page encoding. Without this, you need put
`<%@ page language=“java” contentType=“text/html; charset=UTF-8” pageEncoding=“UTF-8”%>`
in every JSP pages.


```
    <jsp-config>
        <jsp-property-group>
            <url-pattern>*.jsp</url-pattern>
            <page-encoding>UTF-8</page-encoding>
        </jsp-property-group>
    </jsp-config>
```


