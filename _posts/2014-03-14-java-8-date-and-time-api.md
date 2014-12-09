---
published: true
author: Kent Chiu
layout: post
title: "Java 8 時間、日期 API"
date: 2014-03-14 12:35
comments: true
sharing: true
footer: true
categories: 
---



# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



在做java 8 time轉換傳統的 java.util.Date時，要記住一個大原則，就是java 8的 LocalXXX是沒時區的，所以，要想辦法補上時區之後，之後再轉成Instant，就很容易轉換到Date(雖然整個過程實在說不上**容易**)

* Date -> LocalDate
* Date -> LocalTime
* Date -> LocalDateTime 


``` java
    // 先轉成 Instant
    Instant instant = new Date().toInstant();
    // LocalDateTime 有提供 ofInstant 的factory method
    LocalDateTime localDateTime = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());  //2014-06-06T00:41:19.550 
    // LocalDate,LocalTime 沒有提供 ofInstant，需先轉成 LocalDateTime 後再 toLocalDate() 或 toLocalTime()
    LocalDate localDate  = localDateTime.toLocalDate(); //00:41:19.550
    LocalTime localTime = localDateTime.toLocalTime(); //2014-06-06

```

* LocalDate -> Date
* LocalTime -> Date
* LocalDateTime -> Date


``` java
    LocalDateTime localDateTime = LocalDateTime.now();
    Instant instant = localDateTime.toInstant(ZoneOffset.UTC);
    Date date = Date.from(instant);

```


日期格式


``` java
    LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm")); //2014-06-06 00:52

```

LocalDateTime 是沒有時區資訊的，所以，如果要轉成 epoch (unix time)，就必需要加入時區資訊，沒有加入時區，就無法直接轉換。

LocalDateTime 對人來說，是使用上比較直覺的，不管在世界上任何一個地方 '2014-1-1 12:00:00' 都是指2014年第一天的中午12點，不用去管時區，但，如果把這個時間轉成格林威治時間(GMT)，或世界協調時間(UTC)，就必需加上時區的資料，才會正確，美國紐約的 '2014-1-1 12:00:00'跟台灣的'2014-1-1 12:00:00', 一定是不一樣的，美國紐約是GMT - 4:00，台灣是GMT + 8:00

java的 getMillis() 的是採 unix timesamp(epoch)的設計，所以，如果要轉成 MILL


> [epoch](http://www.computerhope.com/jargon/e/epoch.htm)通常是指從1970/1/1 開始增加至今的秒數，最多可到2038-1-19 03:14:07 (32位元有號數最大值: 2,147,483,647)
> ![img](http://en.wikipedia.org/wiki/Year_2038_problem#mediaviewer/File:Year_2038_problem.gif)


## Resource
- <http://docs.oracle.com/javase/tutorial/datetime/iso/legacy.html>
