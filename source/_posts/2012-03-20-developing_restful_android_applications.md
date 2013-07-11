---
author: Kent Chiu
published: true
layout: post
title: "開發RESTful應用程式"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---




andoird程式中，常常會有以下類型的動作方式：

1.  到網路抓取資料
2.  儲存到手機
3.  檢查並更新手機上的資料

像Rss Reader就是很典型的範例，使用者需要在網路有效時對遠端的rss
server同部rss資料，並可以在沒有網路時在local進行離線閱讀。
這類型的程式有幾個困難需要處理

1.  不能在activity進行網路抓資料的動作，因為極可能會block住ui，user無法進行其他操作
2.  抓取資料後，需要在local進行cache(通常是存在SQLite資料庫)
3.  需對資料庫跟遠端的rss進行同步，如果有新的rss，應該進行通知
4.  又不能太過頻繁的進行同步
5.  同步後，使用者在畫面上可以馬上讀取到

解決這些問題，不是在一個activity中全部做完即可，而是需要許多個組件相互配合，才不會讓user有不好的使用經驗，為了解決這些問題，google建議了三個處理這個問題的patterns。詳情請參閱參考資源。

![developing_restful_android_applications_001.png][developing_restful_android_applications_001.png]

![developing_restful_android_applications_002.png][developing_restful_android_applications_002.png]

##### 處理檔案跟資料庫間的關聯

在應用上，db的資料，常常會跟某些特定的某些檔案做關聯，傳統的做法，可能是把檔案位置記在table的某個欄位，或用特殊的命名規則來讓table的record跟檔案名稱可以對應的起來。
Google的提供了另一種方式，主要做法如下：

-   為檔案建立一個專屬的table，裡面包含 “\_id”,”\_data”兩個欄位
-   建立存取檔案專用的content
    uri，該uri可以指定該table的”\_id”，便可取得檔案的相關資訊”\_data”
-   利用ContentResolver.openInputStream直接開啟檔案

這樣做的好處是，ContentProvider的client端(使用ContentProvider
API的程式)不用管實體檔案存那，就算檔案從手機移到SD
card或網路上，都不會影響到client的用法。ContentProvider的client面對的都是相同的Content
URI.

```
User Table                                                  User Picture Table
*-----*------*----------*-------------------------------*  *-----*------------------------------------------*
| _id | name | password | picture                       |  | _id | _data                                    |
*-----*------*----------*-------------------------------*  *-----*------------------------------------------*
|   1 | Kent | xxx      | content://foo.bar.Picture/101 |  | 101 | app_data/data/kent_picture.png           | (user的大頭照在手機記憶體)
|   2 | John | ooo      | content://foo.bar.Picture/102 |  | 102 | http://www.domain.com/john.png           | (user的大頭照在網路上)
|   3 | Bob  | ***      | content://foo.bar.Picture/103 |  | 103 | sd_card/data/profile_picture_of_bob.jpg  | (user的大頭照在sd card上)
*-----*------*----------*-------------------------------*  *-----*------------------------------------------*
```

#### 說明

左邊User Table記錄的是user的基本資料，右邊的User Picture
Table是記錄user的大頭照存放位置。user
table裡並不直接存放大頭照檔案的位置，取而代之的是把大頭的位置放在User
Picture Table，然後透過user table裡的picture欄位存的content uri,
來取得User Picture
Table中的相對row(\_id=101),真正存放檔案的欄位一定要叫**\_data**，當android收到`ContentResolver.openInputStream`的需求時，android會將**\_data**欄位以streaming的方式開啟。

如果直接將檔案實際路徑(ex:app\_data/data/kent\_picture.png)放到user的picture的欄位中，而不是放content
uri，這樣，其他程式則會因沒有權限而無法開啟

Resource
--------

-   [google I/O 2010大會上介紹的處理RESTful Android
    Applications](http://www.google.com/events/io/2010/sessions/developing-RESTful-android-apps.html "http://www.google.com/events/io/2010/sessions/developing-RESTful-android-apps.html")
    [說明影片](http://www.youtube.com/watch?v=xHXn3Kg2IQE&feature=player_embedded "http://www.youtube.com/watch?v=xHXn3Kg2IQE&feature=player_embedded")
    [投影片](http://dl.google.com/googleio/2010/android-developing-RESTful-android-apps.pdf "http://dl.google.com/googleio/2010/android-developing-RESTful-android-apps.pdf")
-   [Programming
    Android](http://programming-android.labs.oreilly.com/ch13.html "http://programming-android.labs.oreilly.com/ch13.html")
    - oreilly線上書籍Programming Android關於android開發RESTful
    client的注意事項


[developing_restful_android_applications_002.png]: /images/wiki/android/developing_restful_android_applications_002.png
[developing_restful_android_applications_001.png]: /images/wiki/android/developing_restful_android_applications_001.png