---
author: Kent Chiu
published: true
layout: post
title: "High Performance MySQL study notes"
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



Lock Control
------------

### Table Locks

-   最基本的鎖定略策略
-   成本最低
-   效能較佳

#### 動作方式

When a client wishes to write to a table (insert, delete,update, etc.),
it acquires a write lock. This keeps all other read and write operations
at bay. When nobody is writing, readers can obtain read locks, which
don’t conflictwith other read locks.

Table locks have variations for good performance in specific situations.
For example, READ LOCAL table locks allow some types of concurrent write
operations. **Write locks also have a higher priority than read locks**,
so a request for a write lock will advance to the front of the lock
queue even if readers are already in the queue (write locks can advance
past read locks in the queue, but read locks cannot advance past write
locks).

### Row Locks

-   最佳的concurrency control
-   花費較高的resources
-   需使用InnoDB and Falcon storage engines

### Implicit and explicit locking

InnoDB會依isolation level自動處理鎖定，但是也可以用SQL Commands來控制



```
    # method 1
    SELECT ... LOCK IN SHARE MODE
    # method 2
    SELECT ... FOR UPDATE

```

雖然MySQL有提供lock table的指令，但能不用就不要用，不論使用何種 storage
engine，除了在transaction中而且autocommit是disabled的，不然不要使用lock
table的指令，因為lock跟transaction的交互是相當的複雜的

Isolation Levels
----------------

-   [READ
    UNCOMMITTED](#read_uncommitted "mysql:high_performance_mysql_study_notes ↵")
-   [READ
    COMMITTED](#read_committed "mysql:high_performance_mysql_study_notes ↵")
-   [REPEATABLE
    READ](#repeatable_read "mysql:high_performance_mysql_study_notes ↵")
-   [SERIALIZABLE](#serializable "mysql:high_performance_mysql_study_notes ↵")

nonrepeatable read : running the same statement twice and see different
data.

phantom reads : when you select some range of rows, another transaction
inserts a new row into the range, and then you select the same range
**again**; **you will then see the new “phantom” row.**

#### READ UNCOMMITTED

-   transactions can view the results of uncommitted transactions.
-   a.k.a dirty read
-   實務應用上用少使用
-   效能也並末比其他的level好很多

#### READ COMMITTED

-   許多資料庫系統預設的level**(但不包含MySQL)**
-   transaction只會看到其他transactions已經commited的資料

#### REPEATABLE READ

-   MySQL預設的level
-   解決[READ
    UNCOMMITTED](#read_uncommitted "mysql:high_performance_mysql_study_notes ↵")的問題
-   但還是有可能發會phantom reads的問題
-   InnoDB and Falcon solve the phantom read problem with multiversion
    concurrency control.

#### SERIALIZABLE

-   解決phantom read problem問題
-   容易發生timeout跟鎖定衝突等問題
-   實務上也比較少用
-   等高等級的Isolation Levels(最耗資源)

### ANSI SQL isolation levels

Isolation level

Dirty reads possible

Nonrepeatable reads possible

Phantom reads possible

Locking reads READ

UNCOMMITTED

Yes

Yes

Yes

No

READ COMMITTED

No

Yes

Yes

No

REPEATABLE READ

No

No

Yes

No

SERIALIZABLE

No

No

No

Yes

Uncategory
----------

#### Transactional Tables

1.  InnoDB
2.  Falcon
3.  PBXT

#### Non-Transactional Tables

1.  MyISAM table
2.  Memory table

non-transactional
table還是可以在transaction中使用，但rollback時無法正常回復

#### Storage Engines

1.  InnoDB
2.  NDB Cluster
3.  Falcon

