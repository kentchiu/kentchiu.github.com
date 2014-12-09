---
author: Kent Chiu
published: true
layout: post
title: "Android 模擬器 101"
date: 2012-12-28
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



Android Emulator是
QEMU-based的應用程式，主要功能是在PC上建立一個虛擬的Android平台環境，並提供一個GUI的仿手機介面以供操作。
Android
Emulator可以模擬任何Android平台的任何功能，可以模擬手機應用程式執行情形，也可以模擬Native
Code的功能。
模擬器有可客制化功能，可以在runtime時選定要用那一種角析度的LCD，要支援什麼樣的硬體設備(攝影機、錄音功能、簡訊、GPS…)

### 啟動

可以透過EclipseADT啟動，也可以直接從Command Line啟動

#### 透過Eclipse ADT啟動

![android_emulator_101_01.png][android_emulator_101_01.png]

你可以加入許多[啟動參數](http://developer.android.com/guide/developing/tools/emulator.html#startup-options "http://developer.android.com/guide/developing/tools/emulator.html#startup-options")來設定啟動後的環境，
比如說是否啟用debug mode，是否套用特定的skin等。

在Eclipse ADT中加入啟動參數的方法是在menu \> Run Configuration \> Target
tab

![android_emulator_101_03.png][android_emulator_101_03.png]

#### 從Command Line啟動

透過command line啟動的方式如下：


```
emulator -avd AVD2

```

AVD2是上圖的ADV Name，那個是建立**A**ndroid **V**irtual
**D**evice建，其中一個步驟要求輸入的內容

##### 啟動後的畫面

啟動時需要幾秒到幾分鐘的時間(一個完整的OS在VM中啟動，當然要花些時間)，啟動後，你可以一直讓模擬器維持開啟狀況，不必隨著你的程式的關閉而關閉。

![android_emulator_101_02.png][android_emulator_101_02.png]

解析度
======

-   QVGA 320×240, 120dpi, 3.3
-   WQVGA432 432×240, 120dpi, 3.9
-   HVGA 480×320, 160dpi, 3.6
-   WVGA800 800×480, 240dpi, 3.9
-   WVGA854 854×480, 240dpi, 4.1

![](http://cdn3.techbang.com.tw/system/images/23074/original/ex_1_wv1.jpg)

圖片來源
[T客邦](http://www.techbang.com.tw/posts/3053-search-text-to-explain-words-wvga-high-resolution-handheld-devices-necessary?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+techbang+%28T%E5%AE%A2%E9%82%A6+%E6%9C%80%E6%96%B0%E6%96%87%E7%AB%A0%29&utm_content=Google+Reader "http://www.techbang.com.tw/posts/3053-search-text-to-explain-words-wvga-high-resolution-handheld-devices-necessary?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+techbang+%28T%E5%AE%A2%E9%82%A6+%E6%9C%80%E6%96%B0%E6%96%87%E7%AB%A0%29&utm_content=Google+Reader")

在啟動模擬器前，還有一個啟動選項(Launch Options)畫面，將scale to real
size勾選，可以讓模擬器瑩幕的大小跟實際在手機上看到比較接近。
這樣可以確定實際按鍵的大小，比較容易評量會不會因為按鍵太小而不容易操作。

![android_emulator_101_004.png][android_emulator_101_004.png]

Resources
=========

-   [Android Emulator](http://developer.android.com/guide/developing/tools/emulator.html "http://developer.android.com/guide/developing/tools/emulator.html")
-   [Android Virtual Devices](http://developer.android.com/guide/developing/tools/avd.html "http://developer.android.com/guide/developing/tools/avd.html")


[android_emulator_101_01.png]: http://blog.kent-chiu.com/images/2012-12-28/android_emulator_101_01.png
[android_emulator_101_03.png]: http://blog.kent-chiu.com/images/2012-12-28/android_emulator_101_03.png
[android_emulator_101_02.png]: http://blog.kent-chiu.com/images/2012-12-28/android_emulator_101_02.png
[android_emulator_101_004.png]: http://blog.kent-chiu.com/images/2012-12-28/android_emulator_101_004.png
