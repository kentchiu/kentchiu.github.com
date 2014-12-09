---
author: Kent Chiu
published: true
layout: post
title: "在Emulator裡使用Android Market"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
  - emulator
---



1.  取的1.6的版Android SDK
2.  建立AVD(假設命名為myAVD)，target設定為1.6版的SDK
3.  去[htc
    developer](http://developer.htc.com/adp.html "http://developer.htc.com/adp.html")下載ROM^[1)](#fn__1)^
    signed-dream\_devphone\_userdebug-img-14721.zip
4.  解壓ZIP後，把system.img檔copy到
    USER\_HOME^[2)](#fn__2)^底下有一個`.android`目錄，裡面有一個剛剛命名的AVD(以我的帳號來說，它是在C:\\Users\\Kent\\.android\\avd\\myAVD.avd\\目錄)
5.  重新啟動Emulator即可看到 Android Market的icon在主畫面上
6.  用google (Gmail)的帳號登入market即可download apps

很多需要硬體支援的功能，Emulator還是沒辦法用

Resources
=========

-   [http://www.51testing.com/?uid-49689-action-viewspace-itemid-212224](http://www.51testing.com/?uid-49689-action-viewspace-itemid-212224 "http://www.51testing.com/?uid-49689-action-viewspace-itemid-212224")
    - 本篇文章主要參考這裡
-   [http://blog.23corner.com/2010/06/17/%E5%9C%A8-android-emulator-%E4%B8%AD%E4%BD%BF%E7%94%A8-android-market-%E7%9A%84%E6%96%B9%E6%B3%95/](http://blog.23corner.com/2010/06/17/%E5%9C%A8-android-emulator-%E4%B8%AD%E4%BD%BF%E7%94%A8-android-market-%E7%9A%84%E6%96%B9%E6%B3%95/ "http://blog.23corner.com/2010/06/17/%E5%9C%A8-android-emulator-%E4%B8%AD%E4%BD%BF%E7%94%A8-android-market-%E7%9A%84%E6%96%B9%E6%B3%95/")
    -
    這篇講到更多的設定方式，但我用1.6版的ROM，只要照我上面寫的流程，就可以使用了



^[1)](#fnt__1)^ 我下的是1.6版的system.img

^[2)](#fnt__2)^ 登入OS的使用者帳號