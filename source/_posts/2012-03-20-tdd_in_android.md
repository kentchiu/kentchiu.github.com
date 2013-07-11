---
author: Kent Chiu
published: true
layout: post
title: "在Android進行TDD式的開發"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
  - testing
  - tdd
---



網路上的android的testing教學，基本上都不太算是真正的unit
testing，而比較像是Integration
Testing(整合測試)，因為大多都需要在模擬器或實機中執行測試，這樣便與環境相關。

目前找到最能接近[Test Driven
Development](http://en.wikipedia.org/wiki/Test%20Driven%20Development "http://en.wikipedia.org/wiki/Test Driven Development")的方式，是使用[Robolectric](http://pivotal.github.com/robolectric/index.html "http://pivotal.github.com/robolectric/index.html")進行真正的Unit
Test。

### 開發環境設定 (使用ADT)

robolectric的開發環境設定比較不一樣，務必參考它網站上的教學一步一步做，主要的關鍵在Test
Project是一般的java project，透過Eclipse Link
Folder的功能，把測試程式link到Test Project。

照[網站上的教學](http://pivotal.github.com/robolectric/eclipse-quick-start.html "http://pivotal.github.com/robolectric/eclipse-quick-start.html")做完後，會有一個試車的動作，如果run起來有出exception，可能sdk版本沒設好，1.0-rc版的Robolectric，預設是sdk
version是9
，如果沒有這一版本的sdk，就會出錯，只要設定**uses-sdk**即可改變Robolectric預設的版本。


```
      <uses-sdk android:targetSdkVersion="10" android:minSdkVersion="10"></uses-sdk>
```

### 開發環境設定 (使用Maven)

不太建議用maven進行android的開發，maven的一些預設的設定，跟ADT產生出來的專案結構不太一樣，而且使用上也比ADT麻煩多了，除非有特殊需求(ex:android
project要上CI)，不然真的是不建議。

我自已是會開一個maven
base的專案，不過主要目的是為了要抓相關的lib，實際開發上，還是直接用ADT，而且不掛任何maven的plugins(eclipse的plugin，不是maven本身的plugin)

### Database 測試

Database的測試需要特別注意，robolectric裡的Shadow
Database是用H2實作，而非SQLite，而且，H2會使用in-momery的方式建立table，所以，可能在同一個test的case的同一個method，你沒也辦法去insert一筆資料後，再透過query把它找出來，目前正在尋找適當的solution
(也有可能是我的使用方式有錯)

整個專案下做下來，用robolectric遇到了不少問題，讓我有IOC
container流行前做TDD的那種痛苦，很多測試都很難進行，寫測寫變成一種痛苦，而不是快樂。

主要的原因在於robolectric的原理是用shadow
object，很多行為都沒有真正進行實作，都是傳回物件的預設值，裡面沒有任何
behaviors, 而且又沒提供一個很容易的方式去動態的改變這種shadow
object的動作方式，造成測試上的困擾。

Resource
========

-   [http://pivotal.github.com/robolectric/index.html](http://pivotal.github.com/robolectric/index.html "http://pivotal.github.com/robolectric/index.html")



