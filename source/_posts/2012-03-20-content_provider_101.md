---
author: Kent Chiu
published: true
layout: post
title: "Content Provider 101"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---




基本觀念

1.  永遠不要自行創建ContentProvider的實例(instance)，系統會自動建立所需的instances
2.  透過ContentResolver來操作ContentProvider，ContentProvider可透過`android.content.Context.getContentResolver()`取得，Application,
    Activity, Service…都是繼承自Context
3.  繼承自ContentProvider
4.  定義CONTENT\_URI `public static final Uri CONTENT_URI`
    -   通常為客制的ContentProvider的完整名稱(pcakge name + class name,
        全小寫)

5.  如果content

Data Model
----------

ContentProvider可視為一個[DAO](http://en.wikipedia.org/wiki/Data_access_object "http://en.wikipedia.org/wiki/Data_access_object")(Data
Access
Object)，它提供的是對資料層的存取方法，但不包含資料本身的結構，所以，通常它操作的對象會是
一個Domain Object, Domain Object所描述的就是資料的結構。

Domain Object的結構，通常會跟資料庫的table對應。

在android，Domain
Object在實作上，有一些潛規格必需遵守，這樣才會讓我們實作的ContentProvider用法與官方的用法比較一致。

Content Uri格式
---------------

![content_provider_101_001.png][content_provider_101_001.png]

Content為android辨識資源的方式，它有一定的格式，上圖說明一個完整的Content
Uri，主要可以分為四個部分：

1.  A : schema部份，一定為content，表示這個uri是要被content
    provider處理的
2.  B :
    authority部份，用來做權限管控的，通常為客制的ContentProvider的完整名稱(pcakge
    name + class name, 全小寫)
3.  C : path部份，可以是一或多個區段，像是 “land/bus”, “land/train”,
    “sea/ship”
4.  D : id部份，資料庫裡的資料的\_ID

更新資料狀態到View上
--------------------

當androiod送一個元件內的狀態或內容改變，有時需同步更新到畫面上，比如說，
我們有一個雲端版的音樂播放器，在首頁只會列出專輯，點選專輯後會列出曲目，點選典目後，會從線上直接播發音樂，並同時抓取線上歌詞庫裡的歌詞。
當音樂開始播發時，歌詞可以還沒下載完成(用非同步下載，以免阻塞UI)，所以，我們在播發器上還看不到歌詞，一旦歌詞下載完成後，播發器上就要自動的開始同步播放歌詞。音檔跟歌詞都必需被保留在手機上，以便下次播放時，可以省去下載的動作。

上面的劇情中，播放器假設是透過一個listview元件顯示一行一行的歌詞，那麼，當歌曲第一次播放時，歌詞還未被下載，所以，listview上是空的，歌曲可能前奏還沒播完，歌詞就被下載完成了，此時，需要一個機制去通知播放器上的顯示歌詞的listview，讓listview
reload歌詞。

以上面動作，我們把它叫做\*新資料抵達\*的劇情，所有存在雲端，但只要還沒下載到手機的資料歌詞，都是新資料。
通當，我們會透過非同步(在worker
thread進行)下載歌詞，當歌詞下載完成時，需要通知UI的歌詞顯示器(ListView)進行
畫面更新，將剛下載到的歌詞顯示到畫面上。

要達到以上的動作，處理的順序大致如下：

1.  利用[Android
    背景作業處理](http://wiki.kent-chiu.com/doku.php?id=android:background_processing "android:background_processing")進行歌詞下載
2.  下載完歌詞後，透過ContentResolver.notifyChange()發出\*新資料抵達\*通知
3.  ListView在onCreate()時，需透過ContentResolver.registerContentObserver()註冊一個ContentObserver來觀察是否有\*新資料抵達\*通知
4.  通\*新資料抵達\*通知發出後，ContentObserver.onChange()會被觸發
5.  在ContentObserver.onChange()在執行adapter.notifyDataSetChanged(),顯示歌詞的ListView便會自動refresh

### Where Clause

ContentProvider.query((Uri uri, String[] projection, String selection,
String[] selectionArgs, String
sortOrder)可用來查詢資料，但當需要查詢的資料只有一筆時，而且知道確切的id，那麼
上面的method有兩種用法

1.  uri採用帶有id的格式
2.  uri採用不帶有id的格式，透過在Where Clause(selection指定id)

uri採用帶有id的格式

```
query("content://myauth/book/1", null, null,null, null)
```

uri採用不帶有id的格式，透過在Where Clause(selection指定id)

```
query("content://myauth/book/1", null, "BOOK_ID=?", new String[]{"1"}, null)
  
```

這兩種方式都可以正確取得book1的資料，但那一種比較好呢？一般來說，大部份情況，應該用uri帶有id的取得單筆資料，但特殊的情形下也可以透過where
clause指定id的方式取的資料。

### Ref

-   [官網說明](http://developer.android.com/guide/topics/providers/content-providers.html "http://developer.android.com/guide/topics/providers/content-providers.html")
-   [http://stackoverflow.com/questions/3544913/android-how-to-use-cursoradapter](http://stackoverflow.com/questions/3544913/android-how-to-use-cursoradapter "http://stackoverflow.com/questions/3544913/android-how-to-use-cursoradapter")
-   [http://mylifewithandroid.blogspot.com/2008/03/observing-content.html](http://mylifewithandroid.blogspot.com/2008/03/observing-content.html "http://mylifewithandroid.blogspot.com/2008/03/observing-content.html")


[content_provider_101_001.png]: /images/wiki/android/content_provider_101_001.png