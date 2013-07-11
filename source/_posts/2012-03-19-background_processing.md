---
author: Kent Chiu
published: true
layout: post
title: "Android 背景作業處理"
date: 2012-03-19
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
---




Android的背景作業主要有三種

1.  Thread
    -   HandlerThread

2.  AsyncTask
3.  Handler

後兩者可以做的事情差不多，Handler使用上比較簡單，但code的可讀性比較差，AsyncTask需另外subclass一個新的類別，但可讀性比較好。

Android的操作，只要超過5秒沒回應(或OnCreate()超過10秒)，程式就會被當作無回應，而系統會丟出ANR(Application
No Response
Exception)。所以，比較耗時費工的動作，都應該考慮用背景作業的方式來完成。常見耗時的工作有。

1.  網路相關的動作
2.  資料庫的動作
3.  檔案操作
4.  複雜的計算

在設計時，應該考慮較糟的情況，而不是開發者當時的環境

Thread
------

Thread跟典型的java
thread一樣，最簡單的背景服務執行方式，只要跟UI無關，用這個最方便。

```
new Thread(new Runnable(){
    @Override
    public void run() {
        // long run job
    }
 
}).start();
```

### Handler

Handler是一種跨thread的溝通機制，可以在一個thread內把訊息丟到Message
Queue，另外一個thread接收訊息，它會跟特定的thread關聯，可以在thread裡可以透過handler向meassage
queue送訊息，也可以從message queue收訊息。 收訊息是由message
queue主動呼叫handle的callback message
[Handler.handleMessage()](http://developer.android.com/reference/android/os/Handler.html#hasMessages(int) "http://developer.android.com/reference/android/os/Handler.html#hasMessages(int)")

![background_processing_002.png][background_processing_002.png]

Thread類別中，並沒有thread執行完成的通知機制，如果想讓thread做完事情後進行通知的動作，那就必須透過handler的訊息發送機制，在thread做完事後，送訊息至訊息佇列。
這樣，想知道這個thread完成的事件的，就可以去訊息佇列接收完成的通知訊息。

thread在start()後，會在建立一個thread來執行run() method，可以在run
method裡的主要工作做完後，用
handler的sendEmptyMessage()，傳送一個任務完成的訊息到訊息佇列。

```
new Thread() {
    public void run() {
        // Download image then continue
        // For Example: downloadImg("file");
        handler.sendEmptyMessage(MSG_DOWNLOADED); // public static final int MSG_DOWNLOADED = 0
    }
}.start();
```

```
private Handler handler = new Handler() {
 
    @Override
    public void handleMessage(Message msg) {
        switch (msg.what) {
            case MSG_DOWNLOADED:
                pd.dismiss();
                // What to do when ready, example:
                openFile();
                break;
            }
        }
    };
```

如果想知道thread是否完成，只需要透過handler的handleMessage這個call back
method來接收任務完成的訊息即可。

Handler是透過[sendMessage()](http://developer.android.com/reference/android/os/Handler.html#sendMessage(android.os.Message) "http://developer.android.com/reference/android/os/Handler.html#sendMessage(android.os.Message)")傳送訊息給message
queue，
傳送的訊息可以是[Message](http://developer.android.com/reference/android/os/Message.html "http://developer.android.com/reference/android/os/Message.html")的物件，但Handler跟Message關係是雙向的，也就是Handler可以傳送Message,
Message中也包含傳送者
Handler，所以，要產生Message時，應該透過[Handler.obtainMessage()](http://developer.android.com/reference/android/os/Handler.html#obtainMessage() "http://developer.android.com/reference/android/os/Handler.html#obtainMessage()")，這樣可以確保
Handler系Message間的關聯的正確性。

### UI Thread V.S Worker Threads

##### UI Thread(Main Thread)

和UI有關的任何更新及操作，都需要在UI thread完成，一般就是 Main
thread。如果UI
thread太久沒回應，就會出現ANR，所以要避免ANR就需要在其他的Thread處理比較耗時的動作。

系統啟動一個應該程式時，預設的情況下，所有元件的所有的動作都是在main
thread(UI
Thread)完成的，而當元件引發系統的callback(像是onKeyDown())時，也都會在UI
Thread中。

關於UI Thread有兩個主要的規則要遵守

1.  不要把UI thread給block住
2.  不要在UI toolkit之外存取UI Thread

##### Worker Threads(Backgroud Threads)

```
public void onClick(View v) {
    new Thread(new Runnable() {
        public void run() {
            Bitmap b = loadImageFromNetwork("http://example.com/image.png");
            mImageView.setImageBitmap(b);
        }
    }).start();
}
```

上面是一個Worker
Thread的簡單例子，`loadImageFromNetwork()`的部份沒有問題，但是`mImageView.setImageBitmap(b)`違反了UI
Thread的第二個規則 : **不要在UI toolkit之外存取UI Thread**

但是，有時，我們又希望在新開出來的Thread也可以把更新ui上的資訊，比如說，我們播放歌曲是在一個新的thread，但又希望歌曲播放的同時，能夠將播放的進度同步在進度條(Progress
Bar).根據最基本的大原則:**和UI有關的任何更新及操作，都需要在UI
thread完成**，在播放歌曲的thread是不能操作ui的動作的。

### UI Thread與Worker Threads間的通訊

Android處理上面的問題的機制是Handler來處理，故有三種方式可以使用

1.  Activity.runOnUiThread(Runnable)
2.  View.post(Runnable)
3.  View.postDelayed(Runnable, long)

上面三種方式，**底層都還是透過Handle的post()實作**


```
        private final Handler mHandler = new Handler();
     
        public final void runOnUiThread(Runnable action) {
            if (Thread.currentThread() != mUiThread) {
                mHandler.post(action);
            } else {
                action.run();
            }
        }
```


```
        public boolean post(Runnable action) {
            Handler handler;
            if (mAttachInfo != null) {
                handler = mAttachInfo.mHandler;
            } else {
                // Assume that post will succeed later
                ViewRoot.getRunQueue().post(action);
                return true;
            }
     
            return handler.post(action);
        }
```


```
        public boolean postDelayed(Runnable action, long delayMillis) {
            Handler handler;
            if (mAttachInfo != null) {
                handler = mAttachInfo.mHandler;
            } else {
                // Assume that post will succeed later
                ViewRoot.getRunQueue().postDelayed(action, delayMillis);
                return true;
            }
     
            return handler.postDelayed(action, delayMillis);
        }
```

我們將原來有問題的code改用View.post(Runnable)的方式來實作

```
public void onClick(View v) {
    new Thread(new Runnable() {  // download是耗時的動作，在另外建立一個thread來執行，所以，下一行的run()，這在個thread.start()後，會在另一個thread(worker thread)執行
        public void run() {
            final Bitmap bitmap = loadImageFromNetwork("http://example.com/image.png");
            mImageView.post(new Runnable() {  // -> 利用ui元件進行post，下面那行的run會執行在ui元件所使用的thread上(Main Thread)
                public void run() {
                    mImageView.setImageBitmap(bitmap);
                }
            });
        }
    }).start();
}
```

這樣做後，上面的操作就是thread-safe的。

注意，上面的code為什麼會有兩個run()，因為，handler的post()**並不會**建立新的thread，而是在呼叫post的thread上thread上執行，所以，我們可以透過view元件的handler來進行post，以確保post裡面的run()
code是在view元件的thread(Main Thread)上做ui操作,
但是耗時的功能，還是要另外開一個thread (worker
thread)來進行，不然，會影效到main thread的效能

上面三個簡便的methods，最好只用在單純的情況下，如果情況比較複雜時，建議還是用獨立的[Handler](#handler "android:background_processing ↵")來處理，更複雜的情況下，採用[AsyncTask](#asynctask "android:background_processing ↵")或許是更適合的solution.

### HandlerThread

Android有另外提供一個繼承自Thread類別的[android.os.HandlerThread](http://developer.android.com/reference/android/os/HandlerThread.html "http://developer.android.com/reference/android/os/HandlerThread.html")類別，這個類別本身就具有Handler功能，可以透過這個類別的提供Handler與該thread進行交互(ex:送訊息，及thread執行完畢的callback)

```
HandlerThread thread = new HandlerThread("myThread");  
thread.start(); // thread 要start後，才能取得looper
Handler handler = new Handler(thread.getLooper()) {
    @Override
    public void handleMessage(Message msg) {
        // 耗時的工作可以在這裡做
    }
};
handler.sendEmptyMessage(0); // handler發送的訊息，會觸發handler.handleMessage()。
```

AsyncTask
---------

AsyncTask用來在UI上執行非同步的工作，它會在worker
thread執行耗時的工作後，自動將結果丟回給UI Thread。

採用AsyncTask的方式是繼承AsyncTask類別，將耗時的動作放在`doInBackground()`，
如果要update ui，將要update ui的動作放在`onPostExecute()`,
`onPostExecute()`是在UI
Thread中執行的，所以**可以安全的在`onPostExecute()`執行UI的操作**。

```
protected abstract Result doInBackground(Params... params);
 
protected void onPostExecute(Result result);
```

`onPostExecute(Result result)`裡的reslut，是`Result doInBackground(Params… params)`執行後的return值，也就是說，在`doInBackground()`做完的結果，可以在`onPostExecute()`中被使用

實作好換AsyncTask，可以透過`execute(Param… params)`執行，下面是一個簡單的範例(取自官網)

```
public void onClick(View v) {
    new DownloadImageTask().execute("http://example.com/image.png");
}
 
private class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {
    /** The system calls this to perform work in a worker thread and
      * delivers it the parameters given to AsyncTask.execute() */
    protected Bitmap doInBackground(String... urls) {
        return loadImageFromNetwork(urls[0]);
    }
 
    /** The system calls this to perform work in the UI thread and delivers
      * the result from doInBackground() */
    protected void onPostExecute(Bitmap result) {
        mImageView.setImageBitmap(result);
    }
}
```

![background_processing_001.png][background_processing_001.png]

執行一個AsyncTask的方式如下，每一個instance只能被執行一次，如果被執行超過一次，會出exception

```
new MyAsyncTask().execute("inputString1", "inputString2");
```

##### Status

另外，可透過`getStatus()`取得狀態，如果想等待AsyncTask直到結束，可透過AsyncTask.get()，呼叫後會一直等到執行結束或timeout

```
public enum Status {
    /**
     * Indicates that the task has not been executed yet.
     */
    PENDING,
    /**
     * Indicates that the task is running.
     */
    RUNNING,
    /**
     * Indicates that {@link AsyncTask#onPostExecute} has finished.
     */
    FINISHED,
}
```

### 大型檔案下載

上面的HttpClinet可用來處理比較小的檔案，但如果要下載大檔案(MB等級),Android
2.3有提供[DownloadManager](http://developer.android.com/reference/android/app/DownloadManager.html "http://developer.android.com/reference/android/app/DownloadManager.html")


```
     DownloadManager.Request dmReq = new DownloadManager.Request( 
        Uri.parse("http://dl-ssl.google.com/android/repository/platform-tools_r01-linux.zip")); 
        dmReq.setTitle("Platform Tools"); 
        dmReq.setDescription("Download for Linux"); 
        dmReq.setAllowedNetworkTypes(DownloadManager.Request.NETWORK_WIFI); //  
     
        // 
        IntentFilter filter = new IntentFilter(DownloadManager.ACTION_DOWNLOAD_COMPLETE); 
        registerReceiver(mReceiver, filter); 
     
        downloadId = dMgr.enqueue(dmReq); 
```


```
      public BroadcastReceiver mReceiver = new BroadcastReceiver() { 
            public void onReceive(Context context, Intent intent) { 
                Bundle extras = intent.getExtras(); 
                long doneDownloadId = 
                    extras.getLong(DownloadManager.EXTRA_DOWNLOAD_ID); 
                tv.setText(tv.getText() + "\nDownload finished (" + 
                    doneDownloadId + ")"); 
                if(downloadId == doneDownloadId) 
                    Log.v(TAG, "Our download has completed."); 
            } 
        };
```

Process lifecycle
-----------------

1.  Foreground process : 有可視化元件，正與使用者互動的process
2.  Visible process : 不在Foreground
    process，但是會影響使用者在畫面上所見的內容(像popup一個dialog時，後面的activity就是屬於visible
    process)
3.  Service process : 秀過startService()啟動的process
4.  Background process : process的activity目前在stop狀態
5.  Empty process : 用在caching，主要是讓activity啟動加快

GC時會由下(Empty process)往上(Foreground process)做GC。

比較耗時的工作，有時不是簡個的開一個thread放進去running就好了，有時啟動另一個service是更佳的解法，尤其是當該操作會拖垮activity時

陷阱
----

[AsyncTask](#asynctask "android:background_processing ↵")被android
frameword限制住最大可同時執行數量了，如果需要比較多個背景作業同時執行時，可以直接使用[Thread](#thread "android:background_processing ↵")，但是過多的thread反而可能會使效能更差，而且多個thread時，可能就得必需進行
任務管理，關於multithreading的併行處理，可以參閱[Concurrency
101](http://wiki.kent-chiu.com/doku.php?id=java:concurrency_101 "java:concurrency_101")

如果是把thread相關的內容宣告成activity的內部類別，那**一定要宣告成static
inner class**, 因為inner class如果不是static的，inner
class會參考到建立它的outter class(activity),而該activity因為thread
refenence著它，而不能順利的被釋放，進而造成memory leaking。

Http Service
------------

通常，在程式裡面，需要用到背景服務的情況，都是為了非同步的從網路取資料(射後不理)，如果要透過http存取資源，請使用HttpClient
這樣會減少很多不必要的麻煩。請下列出幾點HttpClient使用時的注意事項。

1.  避免在Activity裡直接使用HttpClient因為Activity關閉後，HttpClient會隨著關閉
2.  整個應用程式，應該只需建立一份HttpClient而重覆使用(Singleton Patten)
3.  HttpClient用透過MultiThreading的方式執行，不需要特別另外建立thread給它
    (TBD 測一下)
4.  使用HttpClient需注意兩種timeout exceptions，其他的HttpClient會處理掉
    1.  Connection Timeout Exception
    2.  Scoket Timeout Exception

5.  HttpClient使用教學可以參閱[這裡](http://hc.apache.org/httpcomponents-client-ga/tutorial/html/ "http://hc.apache.org/httpcomponents-client-ga/tutorial/html/")

另外Android也可以使用[HttpURLConnection](http://wiki.kent-chiu.com/doku.php?id=android:background_processing "android:background_processing")跟[AndroidHttpClient](http://wiki.kent-chiu.com/doku.php?id=android:background_processing "android:background_processing")，但就必須自已處理thread的部份了


```
    import org.apache.http.HttpVersion; 
    import org.apache.http.client.HttpClient; 
    import org.apache.http.conn.ClientConnectionManager; 
    import org.apache.http.conn.params.ConnManagerParams; 
    import org.apache.http.conn.scheme.PlainSocketFactory; 
    import org.apache.http.conn.scheme.Scheme; 
    import org.apache.http.conn.scheme.SchemeRegistry; 
    import org.apache.http.conn.ssl.SSLSocketFactory; 
    import org.apache.http.impl.client.DefaultHttpClient; 
    import org.apache.http.impl.conn.tsccm.ThreadSafeClientConnManager; 
    import org.apache.http.params.BasicHttpParams; 
    import org.apache.http.params.HttpConnectionParams; 
    import org.apache.http.params.HttpParams; 
    import org.apache.http.params.HttpProtocolParams; 
    import org.apache.http.protocol.HTTP; 
     
    public class CustomHttpClient { 
        private static HttpClient customHttpClient; 
     
        /** A private Constructor prevents instantiation */ 
        private CustomHttpClient() { 
        } 
     
        public static synchronized HttpClient getHttpClient() { 
            if (customHttpClient == null) { 
                HttpParams params = new BasicHttpParams(); 
                HttpProtocolParams.setVersion(params, HttpVersion.HTTP_1_1); 
                HttpProtocolParams.setContentCharset(params, HTTP.DEFAULT_CONTENT_CHARSET); 
                HttpProtocolParams.setUseExpectContinue(params, true); 
                HttpProtocolParams.setUserAgent(params,  "Mozilla/5.0 (Linux; U; Android 2.2.1; en-us; Nexus One Build/FRG83) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1" ); 
                params.setParameter("charset", "UTF-8"); // 要設成utf-8，否則中文的utf-8網頁會出錯 
                ConnManagerParams.setTimeout(params, 1000); 
     
                HttpConnectionParams.setConnectionTimeout(params, 5000); 
                HttpConnectionParams.setSoTimeout(params, 10000); 
     
                SchemeRegistry schReg = new SchemeRegistry(); 
                schReg.register(new Scheme("http",  PlainSocketFactory.getSocketFactory(), 80)); 
                schReg.register(new Scheme("https", SSLSocketFactory.getSocketFactory(), 443)); 
                ClientConnectionManager conMgr = new ThreadSafeClientConnManager(params,schReg); 
                customHttpClient = new DefaultHttpClient(conMgr, params); 
            } 
            return customHttpClient; 
        } 
    } 
```

使用方式


```
    httpClient = CustomHttpClient.getHttpClient()
    try { 
        HttpGet request = new HttpGet("http://www.google.com/"); 
        String page = httpClient.execute(request, new BasicResponseHandler()); 
        System.out.println(page); 
    } catch (IOException e) { 
        // covers: 
        //      ClientProtocolException 
        //      ConnectTimeoutException 
        //      ConnectionPoolTimeoutException 
        //      SocketTimeoutException 
        e.printStackTrace(); 
        // 如果發生SocketTimeout時，可以這樣進行retry
        if (count < retry) {
            // do again
        } else {
            // 
        }
    } 
```

Resource
--------

-   官網上跟thread相關的教學文件
    1.  [Painless
        Threading](http://developer.android.com/resources/articles/painless-threading.html "http://developer.android.com/resources/articles/painless-threading.html")
    2.  [Updating the UI from a
        Timer](http://developer.android.com/resources/articles/timed-ui-updates.html "http://developer.android.com/resources/articles/timed-ui-updates.html")

-   [AsyncTask
    API](http://developer.android.com/reference/android/os/AsyncTask.html "http://developer.android.com/reference/android/os/AsyncTask.html")
-   [Handler
    API](http://developer.android.com/reference/android/os/Handler.html "http://developer.android.com/reference/android/os/Handler.html")
-   [http://www.vogella.de/articles/AndroidPerformance/article.html](http://www.vogella.de/articles/AndroidPerformance/article.html "http://www.vogella.de/articles/AndroidPerformance/article.html")
-   [http://milochen.wordpress.com/2011/03/25/understanding-android-os-src-looperhandler-message-messagequeue/](http://milochen.wordpress.com/2011/03/25/understanding-android-os-src-looperhandler-message-messagequeue/ "http://milochen.wordpress.com/2011/03/25/understanding-android-os-src-looperhandler-message-messagequeue/")


[background_processing_002.png]: http://blog.kent-chiu.com/images/2012-03-19/background_processing_002.png
[background_processing_001.png]: http://blog.kent-chiu.com/images/2012-03-19/background_processing_001.png