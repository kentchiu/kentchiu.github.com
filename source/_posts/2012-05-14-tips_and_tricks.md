---
author: Kent Chiu
layout: post
title: "Android Tips & Tricks"
date: 2012-05-14
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---



### WebView

##### 讓WebView的內容符合手機的畫面

webview常常需要去存取現在的網頁，但大部份的網頁沒有針對moible特別做處理，所以用手機去瀏灠網頁時(透過webview)，通常需要左右的捲動才要辦法看到完整的內容。
解決的方式為


    WebView webview = new WebView(getContext());
    webview.getSettings().setLoadWithOverviewMode(true);
    webview.getSettings().setUseWideViewPort(true);

##### 防止webview開成新的瀏灠器

通常採用webview時，會希望webview
embbeded在客制的APP裡面，但預設的webview開啟url時，會開啟成新的完整的瀏灠器(含url
bar)，可以透過以下的code來防止這個行為。

    WebView webview = new WebView(getContext());
    webview.setWebViewClient(new WebViewClient());

##### 防止Rotate後webview reload

手機在直放改成橫放(或橫放改直放)時，activity會呼叫onCreate()重新建立activity，如果activity裡面webview元件，會造成weview的內容消失。
，可以在AndroidManifest.xml加入如下的設定來避免這種情現


        <activity android:name=".MyActivity" 
                  android:label="@string/app_name" 
                  android:configChanges="orientation|keyboard|keyboardHidden"> 

### Storage

##### Internal Memory

    Context.getFilesDir: /data/data/package_name/files
    Context.getDir: /data/data/package_name/app_
    Context.getCacheDir(): /data/data/package_name/cache
    Environment.getDataDirectory(): /data
    Environment.getDownloadCacheDirectory(): /cache

##### SDCard

    Environment.getExternalStoragePublicDirectory(): /mnt/sdcard

### Activity

#### Launch Mode

Activity Launch Mode是[current
task](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html "http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html")怎麼去啟動的Activity的方式。
可以透過manifest或programming的方式來設定，但有些設定值，只有manifest有提供，沒辦法在runtime設定。

1.  standard
    預設模組，一個activity可以有多個實體，每個實體分屬不同的task，一個task內也可存在多個實體
2.  singleTop
    如果有四個activies啟動在task內是`A→B→C→D`，D在最上面，一般模式下再啟動一次D會變`A→B→C→D→D`，但如果D是singleTop，不論啟動多少次D，都只會是`A→B→C→D`
3.  singleTask
    如果不存在任何task中時，會建新另一個新的task，如果存在task中時，會呼叫其他task中的activity，並讓它成為top
4.  singleInstance
    永遠會建立最新的，但如果目前該activity已經是top，那就不會再建立

#### Handling Runtime Changes

手機翻轉時，會重建整個Activity以便loading最新的配置內容，但有時，我們會希望可以保持Activity的部份狀態，要達到這個目的，可以透以下幾種機制

1.  透過 onSaveInstanceState()
2.  透過 onRetainNonConfigurationInstance()
3.  透過修改配置檔disable掉config change的事件

##### 透過 onSaveInstanceState

這個方式適用到保持可以序列化的值，像原生型別(數值，字串…)
主要是在onSaveInstanceState()被觸發時，把狀態保持在Intent，然後再onCreate()取得原本的狀態

##### 透過 onRetainNonConfigurationInstance

onSaveInstanceState會有一個明顯的限制，就是不能處理太複雜或太大的資料，因為會做序列化，不但耗時，而且會佔不少記憶體空間。
如果要保留比較複雜的資料，可以透過onRetainNonConfigurationInstance()把物件的reference
hold住，之後，可以下次activity
restart時，透過getLastNonConfigurationInstance()取出

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
     
        final MyDataObject data = (MyDataObject) getLastNonConfigurationInstance();
        if (data == null) {
            data = loadMyData();
        }
        ...
    }

要被hold住的物件不能跟context有任何關係，不然不旦會造成錯誤，還會造成memory
leaking

##### 透過修改配置檔disable掉config change的事件

這種方式，是直接告訴activty要忽略某些configuration change


    <activity
     android:configChanges="orientation|keyboardHidden"

一個簡單的翻轉動作，可能同時會觸發多次onConfigurationChanged()，所以，要把相關的設定都關掉，才不會有問題

### List View

##### 下拉後更新 (Pull to refresh)

在許多listview的應用裡可以見到這樣的應用

![android-pull-to-refresh.png][android-pull-to-refresh.png]

這個效果的做法的基本原理，是在listview的header放一個layout，在偵測到listview的scroll事件後，讓layout進行動畫播放，再callback一個AsyncTask去取得資料再更新listview。

GitHub上有兩個這樣的library可用

1.  [https://github.com/johannilsson/android-pulltorefresh](https://github.com/johannilsson/android-pulltorefresh "https://github.com/johannilsson/android-pulltorefresh")
    - 基本的list view pull to refresh
2.  [https://github.com/chrisbanes/Android-PullToRefresh](https://github.com/chrisbanes/Android-PullToRefresh "https://github.com/chrisbanes/Android-PullToRefresh")
    - 這個專案目前比較活躍，另外也提供WebView, GridView,
    PullToRefreshExpandableListView的下拉後更新，程式結構也比較清晰

### TextEdit

##### 讓edit text不會一開始就彈出軟體鍵盤

```
<activity android:name="MyActivity"
          android:windowSoftInputMode="adjustUnspecified|stateHidden"
          android:configChanges="orientation|keyboardHidden"/>
```

[android-pull-to-refresh.png]: http://blog.kent-chiu.com/images/2012-05-14/android-pull-to-refresh.png