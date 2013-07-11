---
author: Kent Chiu
published: true
layout: post
title: "Diff Code Between Web Site and SVN Repos"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - eclipse
---




Dependencies
============

1.  diff function
    ([http://www.holomind.de/phpnet/diff.src.php](http://www.holomind.de/phpnet/diff.src.php "http://www.holomind.de/phpnet/diff.src.php"))
2.  Subversion Client API
    ([http://us3.php.net/manual/en/book.svn.php](http://us3.php.net/manual/en/book.svn.php "http://us3.php.net/manual/en/book.svn.php"))

Requirement
===========

-   比對所有php，lbi，dwt，html檔
-   可以以版本，時間，tag等資訊比對SVN上的code
-   以ecshop內建的table來顯示有差異檔名
    -   以目錄排序
    -   以檔名排序
    -   以修改日期排序
    -   以異動行數排序

-   在檔名click後可以連結到異動細節
-   異動細節以顯眼的方式呈現

User Stories
============

