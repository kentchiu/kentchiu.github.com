---
author: Kent Chiu
published: true
layout: post
title: "Broadcast Receiver 101"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - broadcast_receiver
  - android
  - service
---




Broadcast receivers是Application
Fundamentals中的四大組件之一，主要目的用來在背景接收廣播事件，繼承自BroadcastReceiver，之後再透過在AndroidManifest.xml註冊要聆聽的廣播事件。

當廣播事件抵達時，會執行BroadcastReceiver.onReceive()，而要進行廣播則可利用Context.sendBroadcast()來進行廣播。須注意的是BroadcastReceiver的broadcasted出來的訊息處理是非同步的，也不是一boradcast馬上就會執行，順序也是不確定的，所以，另外有一個Context.sendOrderedBroadcast()可以使用，這個可以保證Broadcast出來的intent的順序性及Priority.

在設計上，可以會面臨Backgroud
Service要以Servicen實作還是要以BroadcastReceiver實作，[這邊](http://developer.android.com/resources/articles/multitasking-android-way.html "http://developer.android.com/resources/articles/multitasking-android-way.html")有篇文章可以參考兩者中那個比較適合。

BroadcastReceiver超過十秒就會ANR，所以，比較長時間的動作應該都是由service做

以下的例子是實作一個執行BroadcastReceiver去聆聽開機完成事件的功能


```
public class MyReceiver extends BroadcastReceiver {
 
    @Override
    public void onReceive(Context context, Intent intent) {
        Intent service = new Intent(context, MyService.class);
        context.startService(service);
    }
}

```



```
        <receiver android:name="MyReciever" >
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
            </intent-filter>
        </receiver>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

```

發送broadcast的方式


```
context.sendBroadcast(intent);

```

如果BroadcastReceiver沒有順利被觸發，可能是因為沒設定權限或者是intent
filter設定錯誤所導致


