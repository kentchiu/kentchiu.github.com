---
author: Kent Chiu
published: true
layout: post
title: "MYSQL Commands"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - mysql
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



Client
------

connect to db



```
    mysql -u username -p

```

Database
--------

list all databases;


```
show databases;

```

swith to database


```
use database_name

```

create database


```
CREATE DATABASE database_name;

```

Table
-----

create table



```
    CREATE TABLE users (
        user_id MEDIUMINT UNSIGNED NOT NULL
        AUTO_INCREMENT,
        first_name VARCHAR(20) NOT NULL,
        last_name VARCHAR(40) NOT NULL,
        email VARCHAR(60) NOT NULL,
        pass CHAR(40) NOT NULL,
        registration_date DATETIME NOT NULL,
        PRIMARY KEY (user_id)
    );

```

insert data


```
INSERT INTO tablename (col1, col2) VALUES (x, y)

```

show all table in current database.


```
SHOW TABLES;

```

Column
------

show all column from users table


```
SHOW COLUMNS FROM users;

```

User
----

Password Change
---------------

FullText Search
---------------

-   need to create fulltext index
-   requires a MyISAM table
-   Using MATCH…KAGAINST in where clause


```
SELECT * FROM tablename WHERE MATCH (columns) AGAINST (terms)

```

-   Inserting records into tables with FULLTEXT indexes can be much
    slower because of the complex index that's required.
-   need to use
    [N-gram](http://en.wikipedia.org/wiki/N-gram "http://en.wikipedia.org/wiki/N-gram")
    in Chinese word.

Get Detail Information Of Column
--------------------------------


```
DESCRIBE table [column];

```

EXPLAIN
-------

show the table deatil.


```
EXPLAIN table;

```

show how query explain


```
EXPLAIN query;

```

LOAD DATA INFILE
----------------

-   data fields in the file must be separated by tabs
-   enclosed in single quotation marks
-   each row must be separated by a newline (\\n).
-   Special characters must be escaped out with a slash (\\)


```
LOAD DATA INFILE 'ewbooks.txt'  INTO TABLE books;

```

Storage Engines
---------------

-   MyISAM
-   MEMORY
-   MERGE
-   ARCHIVE
-   CSV
-   InnoDB

### MyISAM

Used in most situations.

### InnoDB

Used in transaction are importantsuch as for tables storing financial
data or for situations in which INSERTs and SELECTs are being
interleaved, such as online message boards or forums.

Change Password
---------------


```
mysqladmin -u <帳號> -p password <輸入您要的新密碼>

```

在 Ubuntu 或 Debian 的 MySQL 都有一個很特殊的帳號
debian-sys-maint，這個帳號是安裝完 MySQL
後自動產生的，並且擁有所有權限。它的帳號和密碼寫在 /etc/mysql/debian.cnf
裡面。

這個帳號是拿來救急用的

Forget Password
---------------

### Ubuntu or Debian

在 Ubuntu 或 Debian 的 MySQL 都有一個很特殊的帳號
debian-sys-maint，這個帳號是安裝完 MySQL
後自動產生的，並且擁有所有權限。它的帳號和密碼寫在 /etc/mysql/debian.cnf
裡面。為了安全因素，它的權限是 600，也就是只有 root
可以看的到它！由於這個帳號的密碼是隨機產生的，所以並不適合用在網頁中或者其他正式用途上，讀者應該把這個帳號當成救急用或者管理使用。還有一個需要讀者注意的是，讀者不能直接改
debian.cnf
來改這個帳號的密碼，這是沒有效的！實際上它的密碼是放在資料庫，所以你改了該檔案後還是不會同步回資料庫裡面，Ubuntu
把密碼寫在那個檔案純粹是提醒讀者和救急使用的唷！若真的逼不得已要改，必須修改
debian.cnf 後，再用後面講到的方式修改該帳號的密碼到資料庫。

當您不知道原先的密碼時，或者密碼遺失等等，可以使用救急帳號的帳號密碼來修改其他帳號的密碼。當您修改過
debian.cnf 要把他同步回資料庫，也可以使用這種方式。

用急救帳號或者 root 帳號修改其他帳號的 MySQL 密碼：

當使用救急帳號時，其實可以不用輸入帳號和密碼，直接用以下指令引入
debian.cnf


```
mysql --defaults-extra-file=/etc/mysql/debian.cnf 

```

或者


```
mysql -u <救急帳號> -p 
Enter password: <---- 這裡輸入在 Debian.cnf 看到的密碼 
 
mysql> use mysql
mysql> update user set password=password('abcdef') where user='root';
mysql> flush privileges; 
mysql> quit

```

最後一種情形是 debian.cnf 裡面的帳號莫名其妙的不能用，而且 root
帳號的密碼也搞丟了，這時候就比較麻煩了！此時需加上省略帳號認證的參數來略過帳號檢查並進入
MySQL 系統中。

當什們帳號密碼都不知道時，修改 MySQL 密碼的方式：


```
sudo /etc/init.d/mysql stop # 關閉 MySQL 
sudo mysqld_safe --skip-grant-table

```

接下來就和上一個範例一樣，輸入 SQL
指令來修改密碼，唯一不同的是，這個方式進入 MySQL
是不用密碼的！注意，改完後要趕快關閉
MySQL，再用正常的方式重新啟動，不然很容易被入侵！


```
mysqladmin -u root -p shutdown # 關閉 MySQL 伺服器 
Enter password: <---- 輸入剛才改完的密碼 
sudo /etc/init.d/mysql start # 啟動 MySQL 伺服器 

```

### CentOS or Fedora

Stop Server


```
/etc/init.d/mysqld stop

```

Running Server in background mode


```
/usr/bin/mysqld_safe --skip-grant-tables &

```

Login MySQL Server without password


```
mysql --user=root mysql

```

Reset root password


```
update user set Password=PASSWORD('new-password-here') WHERE User='root';
flush privileges;
exit;

```

Restart and login with new password


```
/etc/init.d/mysql start

```

Backup and Restore
------------------

### 資料庫備份

除了透過 phpMyAdmin 來用網頁介面備份資料庫，常見的作法是備份整個
/var/lib/mysql，還原的方式也很簡單，只要拷回去即可，通常會配合 crontab
定時把該目錄 tar 起來。

#### 使用 tar 備份完整資料庫


```
sudo tar zcvf sql_backup.tgz /var/lib/mysql

```

但是透過備份 /var/lib/myqsl 的交換性比較差，有時候換到不同版本的 MySQL
主機可能會不相容，所以這裡筆者比較常把 SQL 資料 dump
到一個文字檔裡面，這裡面還會包含還原資料庫的 SQL
指令，所以很容易帶到不同主機上還原使用。

#### 使用 mysqldump 備份資料庫

1.  使用以下指令可以用 root 身份備份 mysql 這個資料庫到 filename.sql
2.  若要寫在 crontab 裡面使用，那麼輸入 MySQL root
    帳號的密碼是很麻煩的！
3.  此時筆者會配合 –defaults-extra-file=/etc/mysql/debian.cnf
4.  自動輸入帳號密碼。若要備份整個資料庫的話，需要用 –all-databases
    的參數。


```
mysqldump -u root -p --databases mysql > filename.sql

```

以下範例將會引入 debian.cnf
來達成不需要輸入密碼的備份，且將系統上所有的資料庫都備份起來，同時也壓縮成
gz 檔。因為不需要輸入密碼，很適合放在 crontab 裡面自動定時備份。dump
下來的 SQL
原始檔是文字檔，有時候在傳遞時會因為不同系統換行等問題把檔案損毀，所以壓縮起來可以避免這個問題，且大約會讓備份的檔案減少到原來的
30% 左右。

使用 mysqldump 的範例：

1.  以下有兩行，第一行最後面的 \\ 代表換行。如果不打 \\
    也可以，就接下去打，
2.  不用換行。讀者也可以用 bzip 取代 gzip，壓縮率會比較好，不過壓縮
3.  時間會多很多倍！


```
sudo mysqldump --defaults-extra-file=/etc/mysql/debian.cnf --all-databases | gzip > backup.sql.gz

```

察看備份檔內容


```
zcat backup.sql.gz -- MySQL dump 10.10 -- Host: localhost Database: -- Server version 5.0.22-Debian_0ubuntu6.06.2-log

```

### 資料還原

上面使用 mysqldump
下來的資料庫備份檔本質是文字檔，那我們要如何復原到資料庫裡面？其實我們只要使用
mysql -u root -p \< backup.sql
來匯入該文字檔。遇到壓縮過的資料庫檔案，其實道理是一樣的。因為之前舉的例子是壓縮過的，所以在以下範例中筆者使用
zcat 透過 stdin 送到 mysql 這個程式，並且利用 debian.cnf
裡面的帳號密碼來達成不需要輸入密碼的功能。

將壓縮後的 backup.s ql.gz 資料庫備份還原範例：


```
zcat backup.sql.gz | sudo mysql \ --defaults-extra-file=/etc/mysql/debian.cnf    

```

import database


``` bash
mysql -u root -p dm < file.sql

```
