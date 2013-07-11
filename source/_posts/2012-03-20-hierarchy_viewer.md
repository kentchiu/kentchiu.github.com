---
author: Kent Chiu
published: true
layout: post
title: "Android Hierarchy Viewer"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---




Hierarchy Viewer跟Pixel
Prefect兩種工具可以對UI進行除錯跟最佳化，兩種工具都可以使用在手機或模擬器。但需要注意的是，使用前，模擬器必需是已經在運行了，而手機必需是可除錯模式的，
一般手機但多是產品模式，而不是除錯模式，如果手機不是除錯模式，可以把專案DEPLOY到模擬器上做測式。

只有debug這一欄為true，才能使用Hierarchy Viewer tool.

![hierarchy_viewer_001.png][hierarchy_viewer_001.png]

Hierarchy
Viewer可以獨立執行，也可以直接在eclipse使用，獨立使用的方式，執行
ANDROID\_SDK\_HOME\\tools\\hierarchyviewer.bat
(windows開發環境)，如果要在eclipse執行，
可以把模擬器中啟動要進行觀察的專案，再把prospective切換到Hierarchy
Viewer後，便可以在window
viewer中看到執行後的專案，click專案的那個item後，就會出現專案的ui結構圖

![hierarchy_viewer_002.png][hierarchy_viewer_002.png]

Resources
=========

-   [http://developer.android.com/guide/developing/debugging/debugging-ui.html](http://developer.android.com/guide/developing/debugging/debugging-ui.html "http://developer.android.com/guide/developing/debugging/debugging-ui.html")
    - 官網上的說明
-   [Using the Hierarchy
    Viewer](http://mobile.tutsplus.com/tutorials/android/android-tools-using-the-hierarchy-viewer/ "http://mobile.tutsplus.com/tutorials/android/android-tools-using-the-hierarchy-viewer/")


[hierarchy_viewer_001.png]: http://blog.kent-chiu.com/images/2012-03-20/hierarchy_viewer_001.png
[hierarchy_viewer_002.png]: http://blog.kent-chiu.com/images/2012-03-20/hierarchy_viewer_002.png
