---
author: Kent Chiu
published: true
layout: post
title: "Disable X-Window In Ubuntu"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - ubuntu
  - linux
---




Ubuntu uses X Display Manager(xdm、gdm、kdm or wdm) to startup X-Window,
if we want to disabled X-window, we need to disable X Display Manager.

Operations
----------

### check run level



```
    $runlevel
    # output N 2

```

### rename X Display Manager

base on runlevel (2), we can found the file name like **`S30gdm`** under
`/ect/rc2.d`, this is X Display Manager. we rename it to \_s30gdm, and
reboot.

#### file name pattern

S30gdm

-   S for start up, K or shut down
-   30 : priority, maybe difference with yours
-   gdm : a kind of X Display Manager.

resources
---------

-   [ubuntu 修改runlevel,
    不進入X-Window](http://andrewtw.wordpress.com/2007/03/20/ubuntu-%E6%9B%B4%E6%94%B9%E9%96%8B%E6%A9%9F%E9%A0%90%E8%A8%AD-runlevel%EF%BC%8C%E4%B8%8D%E9%96%8B%E5%95%9Fx-window/ "http://andrewtw.wordpress.com/2007/03/20/ubuntu-%E6%9B%B4%E6%94%B9%E9%96%8B%E6%A9%9F%E9%A0%90%E8%A8%AD-runlevel%EF%BC%8C%E4%B8%8D%E9%96%8B%E5%95%9Fx-window/")
-   [切換開機自動進入 X Window
    還是文字模式](http://wiki.linux.org.hk/w/Switch_between_entering_X_Window_mode_and_text_mode_after_boot "http://wiki.linux.org.hk/w/Switch_between_entering_X_Window_mode_and_text_mode_after_boot")
-   [ubuntu下用inittab](http://tw.myblog.yahoo.com/kk-note/article?mid=5&sc=1 "http://tw.myblog.yahoo.com/kk-note/article?mid=5&sc=1")


