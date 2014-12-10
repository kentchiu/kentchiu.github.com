---
author: Kent Chiu
published: true
layout: post
title: "為ECShop加入Logs"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - ecshop
---


Log是很方便的除錯工具(特別是在你不能使用debugger時)，而且可以將log資訊導向檔案，而非web上面。

本文章是在說明如何在ECShop中加入Log功能，以及一些應該注意的事項。

production
environment不應該就log放到web可以access到的資料夾，不然log會被看光光


    require_once 'Log.php';
    $logger = &Log::singleton('file', ROOT_PATH . 'logs/debug.log', 'TEST');



    require_once 'Log.php';
    $logger = &Log::singleton('file', ROOT_PATH . 'logs/debug.log', 'TEST');

-   admin跟商城的init.php是分開的，所以兩邊都要放
-   \$logger的宣告要放在init.php前面一點的地方，因為include\_path會被改寫，改寫後，會發生找不到log定義的問題

Resources
=========

-   [pear設定方式](http://wiki.kent-chiu.com/doku.php?id=php:pear_101 "php:pear_101")
-   [pear
    Log簡介](http://wiki.kent-chiu.com/doku.php?id=php:log_101 "php:log_101")
-   [Show Log in Eclipse
    Console](http://wiki.kent-chiu.com/doku.php?id=php:show_log_in_eclipse_console "php:show_log_in_eclipse_console")

