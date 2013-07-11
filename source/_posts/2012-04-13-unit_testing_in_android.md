---
author: Kent Chiu
published: true
layout: post
title: "Android Unit Test 101"
date: 2012-04-13
comments: true
external-url:
sharing: true
footer: true
categories:
  - android
  - testing
---



這邊並不算真正的unit
test，因為整個測試還是需要模擬器或實機執行，而且很依賴測試環境的context，如果有需要
也是會去連網路或資料庫。

準備工作
========

要進行Android Testing通常是把test cases建立在另一個Test
Project而不是像一般的java(maven base)目錄結構一樣
在同一個project建立test folder.

Testing project可透過eclipse的android project
wizard建立，建立後記得先把production project裡用到的lib export出來給test
project使用

![unit_testing_in_android_003.png][unit_testing_in_android_003.png]

而測試專案只需要放只有測試時會用到的lib，因為Tesing
Project有reference到Production Project，所以會一併引用Production Project
export 出來的lib。

一般習慣把被測的程式的專案叫Production
project(或[SUT](http://xunitpatterns.com/SUT.html "http://xunitpatterns.com/SUT.html"))，
而測試程式專案叫Testing Project

Android測試架構
===============

-   測試方式是透過建立測試專案來測試待測專案([SUT](http://xunitpatterns.com/SUT.html "http://xunitpatterns.com/SUT.html"))
-   需啟動Android
    OS，所以是屬於整合測試，而不是單元測試(像是eclipse的plugin測試方式)
-   需繼承特定的TestCase以便可以取得一些測試的基本功能

![](http://developer.android.com/images/testing/android_test_framework.png)

Android應用程式跟測試程式透過Instrumentation Test
Runner執行在同一個Process, 所以，在執行測試時，需透過Instrumentation
Test Runner。

![unit_testing_in_android_001.png][unit_testing_in_android_001.png]

測試分為兩大類

1.  Instrumentation Testing
2.  Android Testing

### Instrumentation Testing

這類型的testing會啟動instrumentation Test Runner。
Instrumentation可以使用instrumentation framework，有了Instrumentation
framework，可以透過Android的事件處理系統傳送訊息給Android應用程式，以便於模擬像”收到mail”，”收到簡訊”，”有電話calling進來”等等的動作，或者是對UI進行自動化測試。

```
TestCase
 |
 `- InstrumentationTestCase
    |
    `- ActivityTestCase
    |  |
    |  `- ActivityInstrumentationTestCase<T>
    |  |
    |  `- ActivityInstrumentationTestCase2<T>
    |  |
    |  `- ActivityUnitTestCase<T>
    |
    `- ProviderTestCase<T>
    |
    `- SingleLaunchActivityTestCase<T>
    |
    `- SyncBaseInstrumentation
```

### Android Testing

提供Context給測試程式

以下的TestCase提供跟Android應用程式相關的Context，透過Context，可以取到Android的檔案系統，資料庫，資源檔…。

```
TestCase
 |
 `- AndroidTestCase
    |
    `- ProviderTestCase2<T>
    |
    `- ServiceTestCase<T>
    |
    `- ApplicationTestCase<T>
```

XxxTestCase2\<T\>中的`tempalte <T>`在使用時要宣告成要被測試的class，就像泛型容器的用法一樣
ex: `List<MyClass>`

### AndroidTestCase Or InstrumentationTestCase

##### AndroidTestCase V.S InstrumentationTestCase

如果測試的對象是ui元件的話，那就必需要用InstrumentationTestCase體系的test
cases，如果要測試non-UI的功能，像permissions之類的功能，那使用AndroidTestCase體系的的test
case即可。
@UiThreadTest(android.test.UiThreadTest)是使用在InstrumentationTestCase的test
method上，用來標明該method會存取UI元件，需要在main thread執行。
或者是，測試時需要使用到test project裡才有的resource(assets, res, …
目錄下的東西)，那就得使用InstrumentationTestCase，因為InstrumentationTestCase才會去處理test
project下的AndroidManifest.xml。

元件測試或應用程式測試，元件測試主要是以下三種

1.  [Activity
    Testing](http://developer.android.com/guide/topics/testing/activity_testing.html "http://developer.android.com/guide/topics/testing/activity_testing.html")
2.  [Content Provider
    Testing](http://developer.android.com/guide/topics/testing/contentprovider_testing.html "http://developer.android.com/guide/topics/testing/contentprovider_testing.html")
3.  [Service
    Testing](http://developer.android.com/guide/topics/testing/service_testing.html "http://developer.android.com/guide/topics/testing/service_testing.html")

而應該程式測試測是測試[Application](http://developer.android.com/reference/android/app/Application.html "http://developer.android.com/reference/android/app/Application.html")

1.  [ApplicationTestCase](http://developer.android.com/guide/topics/testing/service_testing.html "http://developer.android.com/guide/topics/testing/service_testing.html")

[TouchUtils](http://developer.android.com/reference/android/test/TouchUtils.html "http://developer.android.com/reference/android/test/TouchUtils.html")
(android.test.TouchUtils)可用來產生touch
events，通常在InstrumentationTestCase or
ActivityInstrumentationTestCase2中使用。

TestCases
=========

-   [AndroidTestCase](#androidtestcase "android:unit_testing_in_android ↵")
    1.  [ProviderTestCase2](#providertestcase2 "android:unit_testing_in_android ↵")
    2.  [ServiceTestCase](#servicetestcase "android:unit_testing_in_android ↵")
    3.  [ApplicationTestCase](#applicationtestcase "android:unit_testing_in_android ↵")

-   [InstrumentationTestCase](#instrumentationtestcase "android:unit_testing_in_android ↵")
    1.  [ActivityTestCase](#activitytestcase "android:unit_testing_in_android ↵")
        1.  [ActivityInstrumentationTestCase](#activityinstrumentationtestcase "android:unit_testing_in_android ↵")
        2.  [ActivityInstrumentationTestCase2](#activityinstrumentationtestcase2 "android:unit_testing_in_android ↵")
        3.  [ActivityUnitTestCase](#activityunittestcase "android:unit_testing_in_android ↵")

    2.  [ProviderTestCase](#providertestcase "android:unit_testing_in_android ↵")
    3.  [SingleLaunchActivityTestCase](#singlelaunchactivitytestcase "android:unit_testing_in_android ↵")
    4.  [SyncBaseInstrumentation](#syncbaseinstrumentation "android:unit_testing_in_android ↵")

### AndroidTestCase

提供context是在Android
Framework中最重要的一個物件，在許多地方都會用到它，`這邊的context是test project的context`。
AndroidTestCase啟動時，會先做自我檢測，先驗証測試環境是否ready to
use.有提供幾個預先定義好的assert.


### ProviderTestCase2

測試Provider的test
case,如果要測試資料而非UI的話，那測試的對象應該是[Content
Provider](http://developer.android.com/guide/topics/providers/content-providers.html "http://developer.android.com/guide/topics/providers/content-providers.html")及Content
Provider用到的相關物件。

Android Test Framework有提供ProviderTestCase2可以Content
Provider進行獨立的測試。使用方式如下：

```
public class MyContentProviderTest extends ProviderTestCase2<MyContentProvider> {
 
    public MyContentProviderTest() {
        super(MyContentProvider.class, MyContentProvider.class.getName());
    }
 
    @Override
    protected void setUp() throws Exception {
        super.setUp();
    }
 
    public void testInsert() throws Exception {
        ContentValues values = new ContentValues();
        values.put("name", "foo");
        getProvider().insert("context://xxx/xx", values);
        // do assert here
    }
 
    public void testQuery() throws Exception {
        getProvider().query("context://xxx/xx", null, null, null, null);
        // do assert here
    }
}
```

此例中，測是對象為MyContentProvider,使用上要注意的有：

1.  Testcase要繼承自[ProviderTestCase2](http://developer.android.com/reference/android/test/ProviderTestCase2.html "http://developer.android.com/reference/android/test/ProviderTestCase2.html")
    -   可以將`ProviderTestCase2<T>`的template
        \<T\>指定成對象為MyContentProvider，這樣可以testcase內就會引用MyContentProvider而非ContentProvider

2.  必須要建立一個無參數的建構子(Constructor)
3.  Constructor裡必須呼叫`super(MyContentProvider.class, MyContentProvider.class.getName());`
    -   因為MyContentProvider的Authority為`MyContentProvider.class.getName()`

4.  Testcase中透過getProvider()來取得MyContentProvider的instance，而不是自已創建MyContentProvider的instance

### ServiceTestCase

ServiceTestCase可以用來測試Android Service,ServiceTestCase

![unit_testing_in_android_002.png][unit_testing_in_android_002.png]

基本的使用方式如下：

1.  宣告class時，把`template <T>`換成要被測試的Service(Service Under
    Test)
2.  建立default的constructor，並呼叫parent的constructor，代入class name
3.  setUp()時，ServiceTestCase會將real context儲存到systemContext
4.  透過startService()或bindService()啟動service
    (呼叫service.onCreate())
5.  test case的tearDown()會stop 跟 destroy service
6.  如果有需要的話，可以透過setCotext()或setApplication()換掉test
    case裡的context跟application.

ServiceTestCase預設是使用MockApplication + Test Proejct的Context


```
    public abstract class ServiceTestCase<T extends Service> extends AndroidTestCase {
        protected void setupService() {
            mService = null;
            try {
                mService = mServiceClass.newInstance();
            } catch (Exception e) {
                assertNotNull(mService);
            }
            if (getApplication() == null) {
                setApplication(new MockApplication());  // 使用mock版的Application()，而不是真正的
            }
            mService.attach(getContext(), null,  mServiceClass.getName(), null, getApplication(), null);
            assertNotNull(mService);
            mServiceId = new Random().nextInt();
            mServiceAttached = true;
        }
    } 
```

Android有提供[RenamingDelegatingContext](#renamingdelegatingcontext "android:unit_testing_in_android ↵"),
[ContextWrapper](#contextwrapper "android:unit_testing_in_android ↵"),
[IsolatedContext](#isolatedcontext "android:unit_testing_in_android ↵")跟[MockContext](#mockcontext "android:unit_testing_in_android ↵")來換原MockContext，也可以自已Mock一個

```
public class MyServiceTest extends ServiceTestCase<MyService> {
    public MyServiceTest() {
        super(MyService.class);
    }
}
```

![unit_testing_in_android_002.png][unit_testing_in_android_002.png]

ServiceTestCase提供`getService()`可以取得要被測試的service，`getSystemContext()`可以取得test
project context

##### getContext() vs getSystemContext()

在預設的情況下，如果沒有特別去設定context，getContext(),getSystemContext()是同一個instance，但是
如果有設定mock版的context，ex:
`setConext(new MockContext());`，那麼getContext()會取到mock版的，
而getSystemContext()會取到原來的(測試專案的)。

### ApplicationTestCase

測試Application的test case

### InstrumentationTestCase

### ActivityTestCase

### ActivityInstrumentationTestCase

### ActivityInstrumentationTestCase2

### ActivityUnitTestCase

### ProviderTestCase

### SingleLaunchActivityTestCase

### SyncBaseInstrumentation

要測那些東西
============

##### Activity lifecycle events

如果有在Activity lifecycle events像是onCreate(), onResume(), onPause(),
…中保存activity的狀態，那麼應該要對這些methods進行測式，而當
Configuration-changed事件發生時，也需要確定這些event的動作都正確。

![](http://developer.android.com/images/activity_lifecycle.png)

TestCase :
[ActivityInstrumentationTestCase2](#activityinstrumentationtestcase2 "android:unit_testing_in_android ↵")
(大多數情況用這個),
[ActivityTestCase](#activitytestcase "android:unit_testing_in_android ↵"),
[SingleLaunchActivityTestCase](#singlelaunchactivitytestcase "android:unit_testing_in_android ↵")

##### Content Provder

Content
Provder在大部份的情況下，不需要自行建立，也不建議直接建立，所以，Content
Provder在測試時， Insert, Update, Query,
Delete等method，還有getType()這些基本的methods都要被測到，如果沒有必要的話，
儘量不要自已publish額外的method，因為在程式裡，只能取到ContentProvder型別，無法直接取得子型別(除非透過型別轉換，但不建議)，
所以，ContentProvider的子類別，應儘量避免publish額外的API.

TestCase :
[ProviderTestCase2](#providertestcase2 "android:unit_testing_in_android ↵")

##### ListView Adapter

ListView用的Adapter，ListView是UI裡很重要的一個元件，幾乎每個應該程式都會用到，而ListView
Adapter又是ListView中 主要的元件，對Adapter測試的重要性，不言可喻。

```
public class ScriptListAdapterTest extends AndroidTestCase {
 
    public void testExtractWords_none_words() throws Exception {
        ScriptListAdapter adapter = new ScriptListAdapter(getContext(), R.layout.script_list_item, R.id.scriptLine, ImmutableList.<String> of());
        adapter.setRichScript("foo bar");
        assertThat(adapter.extractWord(), Matchers.<String> emptyIterable());
    }
 
    public void testExtractWords_one_word() throws Exception {
        ScriptListAdapter adapter = new ScriptListAdapter(getContext(), R.layout.script_list_item, R.id.scriptLine, ImmutableList.<String> of());
        adapter.setRichScript("foo <b>bar</b> baz");
        Iterable<String> words = adapter.extractWord();
        assertThat(words, hasItem("bar"));
    }
 
    public void testExtractWords_two_words() throws Exception {
        ScriptListAdapter adapter = new ScriptListAdapter(getContext(), R.layout.script_list_item, R.id.scriptLine, ImmutableList.<String> of());
        adapter.setRichScript("<b>foo</b> bar <b>baz</b>");
        Iterable<String> words = adapter.extractWord();
        assertThat(words, hasItems("foo", "baz"));
        assertThat(words, not(hasItem("bar")));
    }
 
    public void testRichText() throws Exception {
        ScriptListAdapter adapter = new ScriptListAdapter(getContext(), R.layout.script_list_item, R.id.scriptLine, ImmutableList.<String> of());
        CharSequence text = adapter.richText("this is a foo bar string", ImmutableList.of("foo", "string"));
        System.out.println(text);
 
    }
}
```

TestCase :
[AndroidTestCase](#androidtestcase "android:unit_testing_in_android ↵")

##### Database & File

我通常會對DatabaseHelper進行測試，以確定table可以正確的被建立或版更

```
public class DatabaseHelperTest extends TestCase {
    private DatabaseHelper    helper;
    private SQLiteDatabase    db;
 
    @Override
    public void tearDown() {
        helper.close();
    }
 
    public void testDictionaryBankTable() throws Exception {
        Cursor c = db.query(DatabaseHelper.DICTIONARY_TABLE_NAME, null, null, null, null, null, null);
        assertEquals(4, c.getColumnCount());
    }
 
    public void testPodcastTable() throws Exception {
        Cursor c = db.query(DatabaseHelper.PODCAST_TABLE_NAME, null, null, null, null, null, null);
        assertEquals(13, c.getColumnCount());
    }
 
    public void testWordBankTable() throws Exception {
        Cursor c = db.query(DatabaseHelper.WORD_BANK_TABLE_NAME, null, null, null, null, null, null);
        assertEquals(2, c.getColumnCount());
    }
 
    @Override
    protected void setUp() throws Exception {
        super.setUp();
        db = SQLiteDatabase.create(null);
        helper = new DatabaseHelper(null, "podcast.db", null);
        helper.onOpen(db);
        helper.onCreate(db);
    }
}
```

TestCase: 沒有用到android 的context，只需用Junit內建的TestCase即可

##### Helper & Utils

Helper跟Utils指的是跟Android比較無關類別，有可能用到網路或檔案系統等的class，可視情況選擇用TestCase,
AndroidTestCase, InstrumentationTestCase
通常我都是先用最簡單的，如果不適合，再找次簡單的。

Test Helper
===========

### Mock Objects

Android Test Framework也有提供一列系的mock
objects，如果在測試時採用Depedency Injection的方式把這些mock
object注入，會讓測試更簡單，更獨立。 這些mock
oject被放在[android.test.mock
package](http://developer.android.com/reference/android/test/mock/package-summary.html "http://developer.android.com/reference/android/test/mock/package-summary.html")內，主要有下列這些mock
objects

1.  MockApplication A mock Application class.
2.  MockContentProvider Mock implementation of ContentProvider.
3.  MockContentResolver An extension of ContentResolver that is designed
    for testing.
4.  MockContext A mock Context class.
5.  MockCursor A mock Cursor class that isolates the test code from real
    Cursor implementation.
6.  MockDialogInterface A mock DialogInterface class.
7.  MockPackageManager A mock PackageManager class.
8.  MockResources A mock Resources class.

這些class採的mock
strategy是stubbing而不是mocking，也就是說，使用時必須overwrite掉要提供資訊的methods

曾經試著將在Android test framework上使用幾個mock
framework，都失敗了，目前都是直接使用Android test
framework內建的mocks，雖不好用但可以接受。

1.  mockito 1.8.5 - 無法通過編譯，殘念
2.  easymock 3 - extension的部份無法通過編譯，也就是只能mock
    interface，殘念
3.  powermock - 還是需要mocktio或easymock的jar，殘念
4.  jmock - 沒試

[這裡](https://sites.google.com/site/androiddevtesting/ "https://sites.google.com/site/androiddevtesting/")有篇文章說明為何大多數的Mock
framework在Android上的無法正常動作， 只要是Mock
Framework有用到CGLib就無法在Dalvik
VM上執行，ex:EasyMock,Mockito都有用到這個lib

2012/3/16 己經有android team的member在將mockito
porting到android上了，或許不久的將來就有android
compatible的mockito可以用了 :) ，詳情 :
[http://code.google.com/p/mockito/issues/detail?id=308](http://code.google.com/p/mockito/issues/detail?id=308 "http://code.google.com/p/mockito/issues/detail?id=308")

### Context For Testing

Context的type hierarchy

![unit_testing_in_android_002.png][unit_testing_in_android_002.png]

跟測試比較相關的有

1.  [ContextWrapper](#contextwrapper "android:unit_testing_in_android ↵")
2.  [IsolatedContext](#isolatedcontext "android:unit_testing_in_android ↵")
3.  [RenamingDelegatingContext](#renamingdelegatingcontext "android:unit_testing_in_android ↵")
4.  [MockContext](#mockcontext "android:unit_testing_in_android ↵")

##### ContextWrapper

嚴格來說，這個並不算是testing專用的，其他不少context也是從這個開始繼承的，他的實作很簡單
，就是把被wrapped的context的delegate給ContextWrapper，所以，每個method大多長這樣

```
/**
 * Proxying implementation of Context that simply delegates all of its calls to
 * another Context.  Can be subclassed to modify behavior without changing
 * the original Context.
 */
public class ContextWrapper extends Context {
    Context mBase;

    @Override
    public AssetManager getAssets() {
        return mBase.getAssets();
    }

    @Override
    public Resources getResources()
    {
        return mBase.getResources();
    }

    @Override
    public PackageManager getPackageManager() {
        return mBase.getPackageManager();
    }
    ...
}
```

##### IsolatedContext

這個Context把跟設備相關的method改掉掉，讓context不與設備直接溝通，但又提供足夠的功能以測試可以進行。
被IsolatedContext改寫掉的methods如下

(圖丟了 XD)

##### RenamingDelegatingContext

正常來說，如果production code裡用到的db 叫
foo.db跟一個檔案叫bar.txt，但測試時，希望不要去動到原來的foo.db,bar.txt
，而是提供一組測試用的foo.db跟bar.txt那就可以用RenamingDelegatingContext處理，可以把測試的命名成
test\_foo.db跟test\_bar.txt，然後跟RenamingDelegatingContext測試用的contxt是以”test\_”當prefix即可，這樣所有測試用的
的檔案名稱跟DB名稱就是原來的名稱加上”test\_”，用以避免prodcution
code跟testing code互相感染。

RenamingDelegatingContext是用來處理檔案都是放在application
project中的情況，但實際使用上，在測試專案上的相同的位置放上一個同檔名的檔案，會比用RenamingDelegatingContext適合，
不過，如果取得測試案專案的resource，就得繼承自InstrumentationTestCase而非AndroidTestCase體系

##### MockContext

Mock版的Context，裡面的implement就是丟出UnsupportedOperationException，所以如果要用這個context，那
測試中有用到的method，就要自行改寫，不然就等著接UnsupportedOperationException。

```
/**
 * A mock {@link android.content.Context} class.  All methods are non-functional and throw 
 * {@link java.lang.UnsupportedOperationException}.  You can use this to inject other dependencies,
 * mocks, or monitors into the classes you are testing.
 */
public class MockContext extends Context {
 
    @Override
    public AssetManager getAssets() {
        throw new UnsupportedOperationException();
    }
 
    @Override
    public Resources getResources() {
        throw new UnsupportedOperationException();
    }
 
    @Override
    public PackageManager getPackageManager() {
        throw new UnsupportedOperationException();
    }
    ...
}
```

### Assertion classes

除了基本的Junit Assertions外，Android Test Framework還提供了

1.  [More
    Asserts](http://developer.android.com/reference/android/test/MoreAsserts.html "http://developer.android.com/reference/android/test/MoreAsserts.html")
    - 一些android testing
    assertions的延伸，不過我覺得[hamcrest](http://wiki.kent-chiu.com/doku.php?id=java:hamcrest_101 "java:hamcrest_101")更好用
2.  [View
    Asserts](http://developer.android.com/reference/android/test/ViewAsserts.html "http://developer.android.com/reference/android/test/ViewAsserts.html")
    - 一些跟view有關的assertions，像是view跟view之間的關係、對齊方式、…

目前Android Test Framework(Android
2.3)還都是基於Junit3,Junit3的assertions在使用上沒有Junit4的來方便(powered
by Hamcrest)，Junit3沒有內建`assertThat()`，
Hamcrest的assertions是一個獨立的lib，所以，還是可以拿來跟Junit3搭著用，但是，Hamrcest的jar檔(1.3-R.C2)丟到Eclipse
ADT時，無法順利通過編譯，[這裡有提供解決的方式](http://wiki.kent-chiu.com/doku.php?id=java:hamcrest_101#hamcrest_android "java:hamcrest_101")
，至於Hamcrest的assertion，可以參閱[Hamcrest網站的教學](http://code.google.com/p/hamcrest/wiki/Tutorial "http://code.google.com/p/hamcrest/wiki/Tutorial")或[這裡](http://wiki.kent-chiu.com/doku.php?id=java:hamcrest_101 "java:hamcrest_101")也有一個簡單的說明。

#### MISC

1.  RenamingDelegating - Product
    code用的是一個名字，測試時用的是另一個名字(檔案或資料庫)，用來避開testing
    code改到product code的內容
2.  ActivityMonitor -
    android.app.Instrumentation.ActivityMonitor是Instrumentation的內部類別，可以用來monitor系統的行為。

如果test proejct跟production project有用到相同的jars，只需由production
project export出jars讓testing project使用即可，如果testing
project有跟production project相同的lib，可能compile time會出錯

Resources
=========

-   [Hello World
    Test](http://developer.android.com/resources/tutorials/testing/helloandroid_test.html "http://developer.android.com/resources/tutorials/testing/helloandroid_test.html")
-   [Testing and
    Instrumentation](http://developer.android.com/guide/topics/testing/testing_android.html "http://developer.android.com/guide/topics/testing/testing_android.html")
-   [Testing In Eclipse, with
    ADT](http://developer.android.com/guide/developing/testing/testing_eclipse.html "http://developer.android.com/guide/developing/testing/testing_eclipse.html")
-   [http://codinghard.wordpress.com/2009/06/04/a-first-look-at-the-android-testing-framework/](http://codinghard.wordpress.com/2009/06/04/a-first-look-at-the-android-testing-framework/ "http://codinghard.wordpress.com/2009/06/04/a-first-look-at-the-android-testing-framework/")
-   [Native
    Driver](http://code.google.com/p/nativedriver/ "http://code.google.com/p/nativedriver/")
    - google官方的ui test
    api，webdriver([selenium](http://code.google.com/p/selenium/ "http://code.google.com/p/selenium/"))
    like的測試工具
-   RenamingDelegatingContext的使用方式可以參考下面的資料
    1.  [http://blog.jayway.com/2011/10/10/using-renamingdelegatingcontext-to-mock-contentresolver-in-android/](http://blog.jayway.com/2011/10/10/using-renamingdelegatingcontext-to-mock-contentresolver-in-android/ "http://blog.jayway.com/2011/10/10/using-renamingdelegatingcontext-to-mock-contentresolver-in-android/")
    2.  [https://docs.google.com/View?id=ddwc44gs\_21737cgtvfj](https://docs.google.com/View?id=ddwc44gs_21737cgtvfj "https://docs.google.com/View?id=ddwc44gs_21737cgtvfj")

-   [http://www.slideshare.net/dtmilano/introduction-to-android-testing](http://www.slideshare.net/dtmilano/introduction-to-android-testing "http://www.slideshare.net/dtmilano/introduction-to-android-testing")
    - android testing的簡介投影片，有每個test case使用上的差異



[unit_testing_in_android_003.png]: http://blog.kent-chiu.com/images/2012-04-13/unit_testing_in_android_003.png
[unit_testing_in_android_001.png]: http://blog.kent-chiu.com/images/2012-04-13/unit_testing_in_android_001.png
[unit_testing_in_android_002.png]: http://blog.kent-chiu.com/images/2012-04-13/unit_testing_in_android_002.png
[unit_testing_in_android_002.png]: http://blog.kent-chiu.com/images/2012-04-13/unit_testing_in_android_002.png
[unit_testing_in_android_002.png]: http://blog.kent-chiu.com/images/2012-04-13/unit_testing_in_android_002.png
