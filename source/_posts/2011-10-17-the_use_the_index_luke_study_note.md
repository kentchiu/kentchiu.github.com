---
author: Kent Chiu
published: true
layout: post
title: "The Use thd index,Luke 學習筆記"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - database
---




這篇的內容，主要是記錄 [Use the
index,Luke](http://use-the-index-luke.com "http://use-the-index-luke.com")
這本線上書籍的學習筆記。 內容大部份取自該網站。

關於 [Use the
index,Luke](http://use-the-index-luke.com "http://use-the-index-luke.com")
這本書：這本書是以Open
Document的方式公佈在網路上，有網頁版及pdf版可以下載，
本書的內容主要描述database
index的方方面面，該書作者認為對一個開發人員來說，對於database的性能調教，只需要瞭解index就很足够了。

Anatomy of an SQL Index
-----------------------

### The Leaf Nodes

![the_use_the_index_luke_study_note_001.png][the_use_the_index_luke_study_note_001.png]

-   大部分的database的index設計多以B-Tree結構實作
-   index本身不為database的一部分,是另外存放在table之外
-   index是在database的block或page上操作
-   index結構小且固定，易於排序或異動操作
-   一個indexed data對應到一個table row，indexed data是排序過的，但table
    data本身是沒有排序過的
-   indexed data的排序不是簡單的由大到小，或由小大而，而是balanced
    search tree (B-Tree)的排序結構

### The Tree

![the_use_the_index_luke_study_note_002.png][the_use_the_index_luke_study_note_002.png]

-   B-Tree的每一個node有兩個以上的值，會由小到大排列
-   B-Tree的next level的節點的最後一個值，會為上一級節點的值
-   同一個level的節點內的值也會由小到大依序排列
-   值可以是重覆的

![the_use_the_index_luke_study_note_003.png][the_use_the_index_luke_study_note_003.png]

-   以據B-Tree，可以很快的找任一個節點
-   以要找值”57”來說，從root開始找的規則如上圖所示
    1.  root找57介於39到83之間，找出下一級(level
        2)的節點的最大節為83的節點
    2.  找到level2的節點為
        46,53,57,83，裡面已有我們要的值”57”，再依上述的規則找level3的節點
    3.  找到level3的節點為
        55,57,57,此節點已經是葉節點(leaf)，所以，即為我們的要節點

一般數百萬的資料，tree的深度約在4\~5層，最糟的情形下，tree的深度也不應該大於7層

### Slow Indexes

即使使用了index，search的動作還是有可能很慢，主要的原因發生在以下兩個操作

1.  Scanning a wider range than intended
2.  Table access

##### Scanning a wider range than intended

像上例中，57的使在leaf
node出現了兩次，這代表如果有需要，database會對這個兩個57的子節點做更多的比對，而這種情形，在index愈大時，愈容易發生

##### Table access

在最糟的情形下，每一個rowid可能分佈在不同的table
block，這表示database需要讀入更多的table
block才能完成操作，另句話說，如果所有的rowid分佈在同一個table
block，那麼只需讀一個table block就可以完成search

The Where Clause
----------------

### The Equals Operator

以下以此table的schema為例

```
CREATE TABLE employees (
   employee_id   NUMBER         NOT NULL,
   first_name    VARCHAR2(1000) NOT NULL,
   last_name     VARCHAR2(1000) NOT NULL,
   date_of_birth DATE           NOT NULL,
   phone_number  VARCHAR2(1000) NOT NULL,
   CONSTRAINT employees_pk PRIMARY KEY (employee_id)
);
```

employee\_id是pk，針對pk做query後得到的execution plan如下

```
SELECT first_name, last_name
  FROM employees
 WHERE employee_id = 123
 
 
-----------------------------------------------------------------
| Id | Operation                   | Name         | Rows | Cost |
-----------------------------------------------------------------
|  0 | SELECT STATEMENT            |              |    1 |    2 |
|  1 |  TABLE ACCESS BY INDEX ROWID| EMPLOYEES    |    1 |    2 |
|* 2 |   INDEX UNIQUE SCAN         | EMPLOYEES_PK |    1 |    1 |
-----------------------------------------------------------------
 
Predicate Information (IDENTIFIED BY operation id):
---------------------------------------------------
   2 - access("EMPLOYEE_ID"=123)
```

由上面的結果可以看到此sql是透過速度最快的*INDEX UNIQUE
SCAN*執行，可以得到唯一的結果，然後需再透過*TABLE ACCESS BY INDEX
ROWID*取得”first\_name”, “last\_name”， 取”first\_name”,
“last\_name”是透過[The Leaf Nodes](#the_leaf_nodes "database:the_use_the_index_luke_study_note")方式，所以速度很快

如果在EMPLOYEE\_ID再加入一個sub ID時，讓employee\_id,
subsidiary\_id成為一個UNIQUE INDEX

```
CREATE UNIQUE INDEX employee_pk 
    ON employees (employee_id, subsidiary_id);
```

當where條件中，同時包含employee\_id, subsidiary\_id, 就會用INDEX UNIQUE
SCAN

```
SELECT first_name, last_name
  FROM employees
 WHERE employee_id   = 123
   AND subsidiary_id = 30;
 
-----------------------------------------------------------------
| Id | Operation                   | Name         | Rows | Cost |
-----------------------------------------------------------------
|  0 | SELECT STATEMENT            |              |    1 |    2 |
|  1 |  TABLE ACCESS BY INDEX ROWID| EMPLOYEES    |    1 |    2 |
|* 2 |   INDEX UNIQUE SCAN         | EMPLOYEES_PK |    1 |    1 |
-----------------------------------------------------------------
 
Predicate Information (IDENTIFIED BY operation id):
---------------------------------------------------
   2 - access("EMPLOYEE_ID"=123 AND "SUBSIDIARY_ID"=30)   
```

但如果where條件中，只用到SUBSIDIARY\_ID，那便會用效率不好的TABLE ACCESS
FULL (Full Table Scan)

```
SELECT first_name, last_name
  FROM employees
 WHERE subsidiary_id = 20;
 
----------------------------------------------------
| Id | Operation         | Name      | Rows | Cost |
----------------------------------------------------
|  0 | SELECT STATEMENT  |           |  110 |  477 |
|* 1 |  TABLE ACCESS FULL| EMPLOYEES |  110 |  477 |
----------------------------------------------------
 
Predicate Information (IDENTIFIED BY operation id):
---------------------------------------------------
   1 - filter("SUBSIDIARY_ID"=20)
```

因為查詢時，是查詢以下的結構

![the_use_the_index_luke_study_note_004.png][the_use_the_index_luke_study_note_004.png]

如果要讓查詢更有效率，可SUBSIDIARY\_ID再為加上的一個的index，就會由FULL
TABLE SCAN變成INDEX RANGE SCAN

```
---------------------------------------------------------------
| Id | Operation                   | Name       | Rows | Cost |
---------------------------------------------------------------
|  0 | SELECT STATEMENT            |            |  110 |   77 |
|  1 |  TABLE ACCESS BY INDEX ROWID| EMPLOYEES  |  110 |   77 |
|* 2 |   INDEX RANGE SCAN          | EMP_SUP_ID |  110 |    1 |
---------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------
   2 - access("SUBSIDIARY_ID"=20)
```

#### Execution Plan分析

```
SELECT first_name, last_name, subsidiary_id, phone_number
  FROM employees
 WHERE last_name  = 'WINAND'
   AND subsidiary_id = 30;
```

如果用以上條件查詢，會產出

```
-----------------------------------------------------------------
| Id | Operation                   | Name         | Rows | Cost |
-----------------------------------------------------------------
|  0 | SELECT STATEMENT            |              |    1 |   30 |
|* 1 |  TABLE ACCESS BY INDEX ROWID| EMPLOYEES    |    1 |   30 |
|* 2 |   INDEX RANGE SCAN          | EMPLOYEES_PK |   40 |    2 |
-----------------------------------------------------------------
 
Predicate Information (IDENTIFIED BY operation id):
---------------------------------------------------
   1 - filter("LAST_NAME"='WINAND')
   2 - access("SUBSIDIARY_ID"=30)
```

我們一步步解說database做了什麼事

1.  第一步是*INDEX RANGE SCAN*它的id是2，對應到”2 -
    access(“SUBSIDIARY\_ID”=30)“，它會在index
    tree，找出第一個”SUBSIDIARY\_ID”=30的leaf node，透過這個leaf
    node可以找出所有相符的資料(可能會有相當多筆，此例中是40筆)
2.  第二步是*TABLE ACCESS BY INDEX
    ROWID*會取出ROWID相對的整個資料列(row)，有了整個資料列，便可執行filter(“LAST\_NAME”='WINAND')的動作。

在原來有index的欄位上做'UPPER''之類的function，會讓查詢變成FULL TABLE
SCAN

用Bind Parameters可以有以下兩個益處

1.  Security 可以避免SQL Injection
2.  Performance Optimizer會重用cache住的sql statement，如果執行的sql
    statement相當的相似。

### Searching For Ranges

SQL的查詢的操作運算子(EX:
\<,\>,between,…)可以執行基於key的查找，而**查詢欄位的順序**對這類查詢的效能，會有巨大的影響。

Execution Plan
--------------

### Oracle Scripts

### PostgreSQL Scripts

要觀查PostgreSQL的Execution Plan只需要在要執行的statement前加上 EXPLAIN

```
EXPLAIN SELECT * FROM MY_TABLE
```

如果SQL Statement中有bind parameter (\$1, \$2, …)，就必需先進行prepare

```
PREPARE stmt(int) AS SELECT $1;
```

然後再EXPLAIN EXECUTE

```
EXPLAIN EXECUTE stmt(1);
```

```
 QUERY PLAN
------------------------------------------
 Result  (cost=0.00..0.01 rows=1 width=0)
```

-   cost
    cost有兩個值，第一個是啟動所需的cost，第二個是執行後，取回全部資料所需的cost，cost是相對的值，沒有一定的單位，各種資料庫的cost有自已計算的方式
-   rows 估計的row count
-   width 預期的row width
    :![FIXME](http://wiki.kent-chiu.com/lib/images/smileys/fixme.gif)

EXPLAIN可以加上ANALYZE，會得到更多的資料，但ANALYZE會真正去執行SQL
COMMAND，所以**當COMMAND是INSERT，UPDAET，DELETE時要特別小心**

```
EXPLAIN ANALYZE EXECUTE stmt(1);
 
                   QUERY PLAN
--------------------------------------------------
 Result  (cost=0.00..0.01 rows=1 width=0)(actual time=0.002..0.002 rows=1 loops=1)
 Total runtime: 0.020 ms
```

### SQLServer Scripts


[the_use_the_index_luke_study_note_003.png]: /images/wiki/database/the_use_the_index_luke_study_note_003.png
[the_use_the_index_luke_study_note_004.png]: /images/wiki/database/the_use_the_index_luke_study_note_004.png
[the_use_the_index_luke_study_note_002.png]: /images/wiki/database/the_use_the_index_luke_study_note_002.png
[the_use_the_index_luke_study_note_001.png]: /images/wiki/database/the_use_the_index_luke_study_note_001.png