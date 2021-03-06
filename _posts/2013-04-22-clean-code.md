---
author: Kent Chiu
layout: post
title: "Clean Code閱讀筆記"
date: 2013-04-22 10:06
comments: true
sharing: true
footer: true
tags: 
  - programming
  - book
  - clean code
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



###### 代碼被讀取的次數遠比寫的次數多

之前只會以為這句是很正常不過的話，而且也認為通常是讀代碼的對象大多是自己，但 Uncle Bob 用一個 Editor Replay (讓編輯器或 IDE 
有類似 Media Player replay 的功能)的例子說明，當在 coding 時，在 method 間的查閱，呼叫，引用…等，這就是在閱讀自己的代碼了。
也就是說，當要寫一個功能時，就一定會不斷的在閱讀自己的代碼了。

###### 童子軍軍規

童子軍有一條軍規是**讓營地比你來時更乾淨**，套用在寫程式上，就是在每次的 checkin ，代碼應該都是要比 checkout 時更 clean

很多人都提到好的程式，不是一開始就規畫出來的，而且每天不斷不斷的重構、改進。重構這些也不用刻意安排時間去做，應該是在每次的 
checkin / checkout 時就順手整理。

## 命名
 

###### 命名要能揭示他的意圖

- WHY  : 要能看出為何存在 
- WHAT : 要能看出做了什麼
- HOW  : 要能看出如何被使用 (是如何被使用，不是如何做)

如果命名時還需要加上額外的注釋，就不會是個好的名字


###### 不要用不夠明確的字

    getActiveAccount()
    getActiveAccounts()
    getActiveAccountInfo()

這三者並無法從名字上區分不同，應該要避免。 (不過個人覺得 account, accounts 是有作用的字，一個代表單數，一個代表複數資料結構)
像是 *Info,Object,Data* 這樣的字，跟 a, an, the 一樣，太含混的字，不應該用來命名

也不用特意在命名時加上型別， ex: NameString, CustomerObject 因為在 IDE 幫助下，已經可以很方便的知道物件的型號了，不需要特別去加上類別資訊

###### 採用技術性的命名方式，而非領域性的命名方式

會去看代碼的，大多是 programmer ，所以應該是用技術性的名字，像是 JobQuery, AccountVisitor 來取名字，而不是領域上的專業術語，如果一定要用到領域上的術語
，那務必讓術語的名字與領域術語能一致。


> 私以為有時使用領域術語命名，會比較直覺，要維護該程式，應該要對該專業領域有所瞭解


#### Class Name

- 不要有像 Info, Data, Processor這樣的字

#### 命名的一致性

- 像 fetch, retrieve, get 意義上相等的字，應該只取一組就好，不要有的 method 是用 get ，有的又用 fetech，還有的用 retrieve 這樣，使用 API 的人，搞不清楚要用那一套
- Driver, Manager, Controller 也是意義上相等的字，應該只取一組就好，因為很難從字面上分辨 DeviceManger 跟 DeviceController 會有什麼不同

> 如果是實作上本身就有差異性 ex: insert 跟 append，那兩個近義字同時使用，是可被接授的


## Functions

- function 愈短愈好，但怎樣的長度叫短呢？ Uncle Martin 認為應該像 Kent Beck 的寫作風格一樣，每個 method 都不超過5行
- 每個 if, else , while statement 的內容，應該要只有一行，可以用 function call (extract to function) 把內容濃縮成一行，
  這樣不但可以讓 method 改小，也可以增加可讀性，也就是說，程式裡只要出現 nest structure 就是一個可以做 extract 的訊號
- *一個 funciton 應該只作一件事*，但何謂一件事呢？如果 function 可以的部份內容可以被 extract 成另一個 funciton ，
  extract 後的 function 是一個完全不同於原來 function 的功能，而不只用另一個方式描述原來的功能，就代表了 function 坐了不止一件事

		public static String renderPageWithSetupsAndTeardowns( PageData pageData, boolean isSuite) throws Exception { 
		  	if (isTestPage(pageData))
				includeSetupAndTeardownPages(pageData, isSuite); 
				return pageData.getHtml();
			}
		}

  如果將上面的 function extract 成 

		public static String renderPageWithSetupsAndTeardowns( PageData pageData, boolean isSuite) throws Exception { 
			return includeSetupsAndTeardownsIfTestPage(pageData, isSuite);
		}
		
		public static String includeSetupsAndTeardownsIfTestPage( PageData pageData, boolean isSuite) throws Exception { 	 
		  	if (isTestPage(pageData))
				includeSetupAndTeardownPages(pageData, isSuite); 
				return pageData.getHtml();
			}
		}

  新的 `includeSetupsAndTeardownsIfTestPage` function 只是在用另外一種方式描述 `renderPageWithSetupsAndTeardowns()` 雖然可以這樣做 extract，
  但這不代表了原來的 `includeSetupsAndTeardownsIfTestPage`  做了不止一件事，因為新的 function 只是另一種方式來描述原來的 function,
  而如果一個 funtion 的內容，分了成幾個斷落 (sections)，顯示的，這也表明了這個 function 做了不止一件事


> 我試著去翻出 Kent Beck 最早期的 junit (3.4) 大多的 function 也都相當的短 (10行以下)， 也有少數比較長的，但也沒超過50行
> 不過，讓每個 function 都小到了極致，那勢必會產生更多的 fucntions 或 classes 而造成 *Divergent Change（發散式變化* 或 *Shotgun Surgery（霰彈式修改)*
> 這部份，可透過package或class的重新組織，並秉持著single responsibility principle (單一職責原則)，通常就不會有太大的問題 

- functon 內的實作抽像等級應該要一致，不要同一個 function 內，有很高階的實作，又有很低階的實作
  高階，低階是指實作的抽象性，像 getHtml() 就是比較高階，而字串相加，就是屬於比較低階的實作
- small function 在取名上也比較容易，因為它只做一件事，就會比較容易給他一個相符的名稱，所以，如果 funciton 在命名上有困難時，也許就是該 funciton 做了不止一件事了


## Arguments

- 參數的數量愈少愈好，如果能沒有參數最好，三個參數應該已是最大值，四個參數應該是特殊狀況了
- *參數數量多，程式的可測性也會降，更多的參數，將需要更多的測試*，因為參數的排列組合的方式會更多，需要更多的測試
- 用布林當參數時，通常代表該 function 可以被切割成兩個 function


#### 回傳值

- 應儘量避免使用回傳值，如果回傳值是為了改變某個狀態，應該要直接改變物件本身

## Exceptions

- 例外處理的 try / catch / finally 本身就是*一件事*，不可做切割
- 例外處理不要值用error code，不然會造成所有人都依賴的這個 error code，要異動時，一定得去異動 error code



## Object And Data Structures

這樣的code，違反了Law of Demeter

	final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();

最好改成這樣

	Options opts = ctxt.getOptions();
	File scratchDir = opts.getScratchDir();
	final String outputDir = scratchDir.getAbsolutePath();

不選，如果一個 function 像這樣包含了太多相關的物件，那就應該考慮是不是有責任歸屬的問題，重新釐清每個物件的責任後，或許會有所改善

## Boundaries

如果需要在物件間傳遞 Map, List, Set 結構時，可以將它包成物件. 

#### Learning tests

使用 third-party lib 是件不同易的事，要對 thrid-party lib 整合更是不容易，我們可以透過讀閱讀過文件後，寫一些簡單來驗証 lib 行為，是不是跟我們所理解的一樣。
這樣的測試叫 Learning tests。

## Testings

- 可讀性對 test code 比 production code 更重要。
- 專業的程式員應該要將測試重構成更具描述性的表達方式
- 每個 test case 裡的 asssertions 應該要儘可能的少，有人甚至主張一個 test case 只能有一個 assertion，至少應該儘量保持 singal concept per test


BDD Given/When/Then test case 的寫法


``` 

	public void testGetPageHierarchyAsXml() throws Exception { 
		givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
		whenRequestIsIssued("root", "type:pages");
		thenResponseShouldBeXML();
	 }


	public void testGetPageHierarchyHasRightTags() throws Exception { 
		givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
		whenRequestIsIssued("root", "type:pages");
		thenResponseShouldContain("<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"); 
	}

```

###### F.I.R.S.T.

- Fast 執行起來要夠快，才會經常去執行
- Independent 獨立，不需要 testing code 及 production code 以外的東西，像是還要手動設定資料庫，或一定要網路連線
- Repeatable 可重覆 (可重現)
- Self-Validating  簡單而明確的指出測式的結果 (red bar / green bar) 跟失敗的原因
- Timely 測試要在 proudction code 之前寫好，而不是寫好了 production code 再來寫測式 ( test first or TDD )


## Classes

Uncle Bob 提倡*報紙代排版*，也就是 functions 不是換按照 scope ( public -> package -> protected -> private )，而是照閱讀順序 (stepdown)，
一般以 plulic scope 的 fucntion 開始，緊接著是依該 public 裡值用到的順序做排列， 這樣會像報紙一樣，以上至下的閱讀。


> 目前要採用這種方式， IDE (eclipse , interllij ) 目前並沒有支援，況且，如果 function 被多個 function 呼叫， 被呼叫的 function 要放那，也是一個問題。
> 目前 ide 都有 function 的 outline 可以快速跳掉某個 function ， eclipse 也有可以直接看另一個被呼叫的 function viewer ，
> 私以為 newspaper format 不是那麼必要。 

#### 保持精簡

Class 應該儘可能的小，但要小到多少？用 function 的數量來計數比是那麼的精確，用檔案大小來計算，更是不容易反應出 class 
的真實大小，在計算 classs 的大小採用的是所讀的*職責數*，在 OOP 五大定理原則裡有一個叫 SPR (Single Responsibility 
Principle), 中文為*單一職責原則*，單一職責原則要求每個 class 應該只有一個責任（只做一件事），所以，我們可以用　class
是否做了太多的事來判斷 class 是不是太大。

> 用比較實務上的方式來解釋*單一職責原則*的話，可以這樣說: 當同一個物件需要異動時，應該都是基於相同的理由


	public class SuperDashboard extends JFrame implements MetaDataUser public Component getLastFocusedComponent()
	public void setLastFocused(Component lastFocused)
	public int getMajorVersionNumber()
	public int getMinorVersionNumber()
	public int getBuildNumber() }

以上面的 SuperDashboard class 來說，它就具備了兩個職責

1. 版本資訊
2. Java Swing的物件結構

所以，當出貨時(理由一)，版本資訊需要異動， focus (GUI 元件的 foucs)異動時，也可能會改變 SuperDashboard，所以這個 class 應該再被細分。


#### 少量的 Large Class V.S. 大量的 Small Class

在設計 class 時，多數人更偏好寫一個很大的 class 而不是 許多的小 classes，因為這樣可以不用在 class見跳來跳去的閱讀，可以在同一個檔
案裡找到所有需要的東西，覺得畫分成小的 class 反而是造成程式可讀性不佳的原兇。其實，**這一定是個誤會**，切成許多單一職責的 classes，
只要透過系統化的分類，不但可以更快的找到需要的功能。

舉例來說，將許多的小零件分門別類的放在工具箱的小抽遞內，一定比全部混放在一個大抽遞更能被快速的找到。所以，不要害怕 class 的切割，做好
SPR，其他的問題，可以透過系統化的組織來解決 class 過多的問題，而且 Small Class 在 reused的效果上也更好，測試上也比較容易。


#### 內聚力

class 的 instance variables 的被 class 內的 methods 引用的次數愈多，表示其內聚力愈強，如果一個類別的內聚力太低時，就應該考慮是否做切割。

> 內聚力太低時，通常也代表 instance variables 數量太多，某些可能只被特定的 methods 引用，那可能就意味這些 methods 應該被
> extract 出去成為另一個獨立的 class， 這樣對兩個 classes 來說，都會有更高的內聚力


## Emergence

Kent Beck's Xp Simplicity Rules:

1. Runs all the tests
2. Contains no duplication
3. Expresses the intent of the programmer
4. Minimizes the number of classes and methods

ref: <http://c2.com/cgi/wiki?XpSimplicityRules>

#### 重構的方向

1. 去除重覆
2. 可讀性
3. 保持最少跟最小的 classes, methods


## Resource

- planetgeek.ch 整理的clean code cheetsheet <http://www.planetgeek.ch/2013/06/05/clean-code-cheat-sheet/> , <a href='http://blog.kent-chiu.com/images/blog/2013-04-22/Clean-Code-V2.2.pdf'>備份檔</a>
