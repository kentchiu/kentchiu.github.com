---
author: Kent Chiu
published: true
layout: post
title: "ListView Style"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
  - listview
---



以下用幾個簡單的步驟來美化ListView

##### 原始

![listview_style_002.png][listview_style_002.png]

##### 結果

![listview_style_003.png][listview_style_003.png]

##### 底圖

如果沒什麼美工底子，可以到[這個](http://www.bgpatterns.com/ "http://www.bgpatterns.com/")網站產生一個簡易的底圖(下載後檔名改成app\_bg.png)

![listview_style_001.png][listview_style_001.png]

##### drawable/app\_background.xm

因為底圖不會剛好跟螢幕大小一樣，所以要透過bitmap讓底圖重覆


```
    <?xml version="1.0" encoding="utf-8"?>
    <bitmap xmlns:android="http://schemas.android.com/apk/res/android"
        android:src="@drawable/app_bg"
        android:tileMode="repeat" />
```

##### values/styles.xml


```
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <style name="app_theme" parent="android:Theme">
            <item name="android:windowBackground">@drawable/app_background</item>
            <item name="android:listViewStyle">@style/TransparentListView</item>
            <item name="android:expandableListViewStyle">@style/TransparentExpandableListView</item>
        </style>
        <style name="TransparentListView" parent="@android:style/Widget.ListView">
            <item name="android:cacheColorHint">@android:color/transparent</item>
        </style>
        <style name="TransparentExpandableListView" parent="@android:style/Widget.ExpandableListView">
            <item name="android:cacheColorHint">@android:color/transparent</item>
        </style>
    </resources>
```

##### AndroidManifest.xml


```
    <application android:theme="@style/app_theme">
```

### 在eclipse預覽

如果一切都很順利的話，可以用eclipse的layout
editor打開任一個activity，然後，右邊的theme可以看到剛剛新增的`app_theme`
可以看到底圖重覆的在activity的背景

![listview_style_004.png][listview_style_004.png]

如果在eclipse看不到預覽，可以執著執行程式，看看emulator或device有沒有效果，或者試著重開eclipse可能就可以看到preview了

Resource
========

1.  [本文主要參考這邊](http://stackoverflow.com/questions/2706913/how-to-make-android-apps-background-image-repeat "http://stackoverflow.com/questions/2706913/how-to-make-android-apps-background-image-repeat")
2.  [http://androidblogger.blogspot.com/2009/01/how-to-have-tiled-background-cont.html](http://androidblogger.blogspot.com/2009/01/how-to-have-tiled-background-cont.html "http://androidblogger.blogspot.com/2009/01/how-to-have-tiled-background-cont.html")
    - 如何設定底圖

[listview_style_002.png]: http://blog.kent-chiu.com/images/2012-03-20/listview_style_002.png
[listview_style_003.png]: http://blog.kent-chiu.com/images/2012-03-20/listview_style_003.png
[listview_style_001.png]: http://blog.kent-chiu.com/images/2012-03-20/listview_style_001.png
[listview_style_004.png]: http://blog.kent-chiu.com/images/2012-03-20/listview_style_004.png
