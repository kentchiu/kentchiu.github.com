---
author: Kent Chiu
published: true
layout: post
title: "Action Bar"
date: 2012-04-20
comments: true
external-url:
sharing: true
footer: true
tags:
  - android
  - actionbar
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



android 3.0+ (API level 11)

![img](http://developer.android.com/design/media/action_bar_basics.png)

1.  具有 tab 的效果，可以在多個 fragments 間切換
2.  具有 options menu 的功能， options menu 在 3.0 之後會用改採 action
    item 的實作呈現

基本的 actionbar
應用，可以參數[這裡](http://developer.android.com/design/patterns/actionbar.html "http://developer.android.com/design/patterns/actionbar.html")

![actionbar_001.png][actionbar_001.png]

##### 移除 ActionBar


```
<activity android:theme="@android:style/Theme.Holo.NoActionBar">

```

移除後，在程式裡面呼叫 `getActionBar()` 會得到 null

![actionbar_002.png][actionbar_002.png]

##### 隱藏 ActionBar


```
ActionBar actionBar = getActionBar();
actionBar.hide();

```

### 建立 Action Item

建立 Action Item 的方式跟之前建立 Option Menu 的方式一樣，只是有些 xml
屬性不一樣


```
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_foobar, menu);
        return true;
    }

```


```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_save"
          android:icon="@drawable/ic_menu_save"
          android:title="@string/menu_save"
          android:showAsAction="ifRoom|withText" />
</menu>

```

主要的差別在 `android:showAsAction` 屬性 **ifRoom**
是指如果空間夠的話，放呈現該item ， **withText** 則是該 item 需不需要
title 。 不過，就算指定了 title ，如果空間不足時，還是 title
還是有可能不會出現，但基本上，把 id, icon, titile 視為必要屬性就好了。

### 建立 Action View


```
    <item
        android:id="@+id/menu_my_action_vew"
        android:actionViewClass="com.kentchiu.MyActionView"
        android:icon="@android:drawable/ic_dialog_alert"
        android:showAsAction="always"
        android:title="my test action view"/>

```


```
package com.kentchiu;
 
import android.content.Context;
import android.view.LayoutInflater;
import android.widget.LinearLayout;
 
public class MyActionView extends LinearLayout {
 
    public MyActionView(Context context) {
        super(context);
        LayoutInflater inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        inflater.inflate(R.layout.action_test, this, true);
    }
}

```

如果需要支援 Collapsible 的動作，就要實作
`android.view.CollapsibleActionView`。

TBC
===

1.  Action Provider
2.  Navigation Tabs
3.  Drop-down Navigation



[actionbar_001.png]: http://blog.kent-chiu.com/images/2012-04-20/actionbar_001.png
[actionbar_002.png]: http://blog.kent-chiu.com/images/2012-04-20/actionbar_002.png