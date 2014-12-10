---
author: Kent Chiu
published: true
layout: post
title: "Android Media Player"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
tags:
  - android
  - media_player
 --


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



##### AndroidMainfest.xml

如果需要從網路下載音訊檔，需要開放`android.permission.INTERNET`權限
	
	<uses-permission android:name="android.permission.INTERNET" />


### 使用流程

##### State Diagram

狀態對[MediaPlayer](http://developer.android.com/reference/android/media/MediaPlayer.html "http://developer.android.com/reference/android/media/MediaPlayer.html")來說是相當重要的，
在不適當的狀態下呼叫不適當的method會導致MediaPlay丟出例常。

![image](http://developer.android.com/images/mediaplayer_state_diagram.gif)

基本上的使用流程如下

1.  透過`create()`建立(idle狀態)
2.  透過`setDataSource()`初始化MediaPlayer (initialized狀態)
3.  透過`prepare()`預備播放 (preparing and prepared狀態)
4.  透過`start()`播放 (started狀態)
5.  之後可以進行播放相關的控制
6.  透過`stop()`停止播放 (stop狀態) 
7.  **沒有要用MediaPlayer時，記得呼叫release()釋放資源** (end狀態)

### 音訊來源

要播放的訊息檔可以來自

1.  Local resources (放程式/raw目錄的檔案，有resource id的)
2.  Internal URIs (內部uri是指可以透過Content Resolver取存的)
3.  External URLs (外部url大多是指網路上的檔案，只能以streaming讀取)


streaming需要先下載到buffer後再進行播放，未下載的部份，無法做位置控制(forward，reverse，seeking)
，用串流播放時，前端通常會實作兩段式的progress
bar，第一段是目前播放的進度，另一段是已下載到buffer的進度。

##### 播放程式內\*有resource id)的音訊檔

這種方式通常是用來播放跟程式中預先定義好的音效，音樂…


    MediaPlayer mediaPlayer = MediaPlayer.create(context, R.raw.sound_file_1);
    mediaPlayer.start(); // no need to call prepare(); create() does that for you


讀取resource是透過create時指定resource
id，這種方式不用自已呼叫prepare(),create()內會自行呼叫prepare()。

##### 播放手機內部音訊檔

這種方式是透過Content URI取得內部的音訊資源


    Uri myUri = ....; // initialize Uri here
    MediaPlayer mediaPlayer = new MediaPlayer();
    mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
    mediaPlayer.setDataSource(getApplicationContext(), myUri);
    mediaPlayer.prepare();
    mediaPlayer.start();

##### 播放網路上的音訊檔

這種方式是透過標準的url來播放網路上的音訊檔


    String url = "http://........"; // your URL here
    MediaPlayer mediaPlayer = new MediaPlayer();
    mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
    mediaPlayer.setDataSource(url);
    mediaPlayer.prepare(); // might take long! (for buffering, etc)
    mediaPlayer.start();

### Preparation

MediaPlayer要能開始播放，必須先進入Prepared狀態。

##### Asynchronous Preparation

進入Prepared狀態會花掉一些時間，因為prepare可能需要抓取及解碼音訊檔(可能需幾秒)，如果Prepared是在main
thread呼叫，會把ui給block住，這樣會影響操作性。
所以MediaPlayer有提供非同步的Preparation方式，也就是在work
thread進行preparing的動作，可以讓block情況減緩一些。

透過prepareAsync()可以進行非同步的preparing，因為是非同步的，所以如果要知道何時會prepared完成，那就必須透過`MediaPlayer.OnPreparedListener`


```
	public class MyService extends Service implements MediaPlayer.OnPreparedListener {
	    private static final ACTION_PLAY = "com.example.action.PLAY";
	    MediaPlayer mMediaPlayer = null;
	 
	    public int onStartCommand(Intent intent, int flags, int startId) {
	        ...
	        if (intent.getAction().equals(ACTION_PLAY)) {
	            mMediaPlayer = ... // initialize it here
	            mMediaPlayer.setOnPreparedListener(this);
	            mMediaPlayer.prepareAsync(); // prepare async to not block main thread
	        }
	    }
	 
	    /** Called when MediaPlayer is ready */
	    public void onPrepared(MediaPlayer player) {
	        player.start();
	    }
	}

```

### 同步播放進度

MediaPlayer本身並不包含UI相關的component，如果要控制MediaPlayer的播放動作，可以透過Button去呼叫播放的API。
其中，有一個比較麻煩的是顯示播放進度的ProgressBar(或SeekBar)，因為progress
bar需要一直不斷跟MediaPlayer進行同步
最直接的方式是寫一個thread，然後透過Handler(WorkerThread不能直接存取UI元件)去取得MediaPlayer目前播放進度的資料，並持續的設定到ProgressBar上。

##### 透過thread同步ProcessBar

像下面code的這樣，但是，實際使用上，發現效能不是很好，progress
bar不管怎麼調，都沒Android內建的[MediaController](http://developer.android.com/reference/android/widget/MediaController.html "http://developer.android.com/reference/android/widget/MediaController.html")好。
去查了一下MediaController的source發現他是用**recursive(遞歸)的方法**處理的。


``` 
	Thread syncSeekBarThread = new Thread(new Runnable() {
	    @Override
	    public void run() {
	        int currentPosition = 0;
	        while (player != null && currentPosition < total) {
	            int currentPosition = mPlayer.getCurrentPosition();
	            Message msg = new Message();
	            msg.what = currentPosition;
	            threadHandler.sendMessage(msg);
	            Thread.sleep(1000);
	        }
	    }
	});
	 
	syncSeekBarThread.start(); // start synchronizing
	 
	 
	private Handler threadHandler = new Handler(){
	    public void handleMessage(Message msg){
	        seekBar.setProgress(msg.what);
	    }
	};

```

##### 透過Handler的recursive同步ProcessBar

主要的重點有

1.  要找一個觸發recursive的好地點，播放器上的play button是一個不錯的位置
2.  handleMessage一但開始接到Message後，就會不斷的送Message給自已
3.  用sendMessageDelayed來計算下次的觸發時間

相當巧妙的解法，難怪大家都建議多看Android的source，改成這樣後，ProgressBar果然有比較平順。


```
	private class SyncHandler extends Handler {
	    @Override
	    public void handleMessage(Message msg) {
	        int current = msg.what;
	        seekBar.setProgress(current);
	        if (!seekbarChangeListener.isDragging()) {
	            Message m = obtainMessage();
	            m.what = player.getCurrentPosition();
	            // Send meesage to Handler itself will fire handleMessage() again with new Message.
	            sendMessageDelayed(m, 1000 - player.getCurrentPosition() % 1000);
	        }
	    }
	}
	 
	private Handler    handler = new SyncHandler();
	 
	/**
	  * strt
	  *
	  */
	public void PlayButtonOnClick() {
	    handler.sendEmptyMessage(player.getCurrentPosition());
	}

```

Resource
========

-   [官網文件](http://developer.android.com/guide/topics/media/mediaplayer.html "http://developer.android.com/guide/topics/media/mediaplayer.html")
-   [API](http://developer.android.com/reference/android/media/MediaPlayer.html "http://developer.android.com/reference/android/media/MediaPlayer.html")
-   [Wake  locks](http://developer.android.com/guide/topics/media/mediaplayer.html#wakelocks "http://developer.android.com/guide/topics/media/mediaplayer.html#wakelocks")
    - 如果要必免待機時，播放器被停掉，可以設定wake mode
-   [Audio  focus](http://developer.android.com/guide/topics/media/mediaplayer.html#audiofocus "http://developer.android.com/guide/topics/media/mediaplayer.html#audiofocus")
    - 如果要處理接到電話或其他事件時，把播放器暫停，可以透過audio focus(2.2+)

