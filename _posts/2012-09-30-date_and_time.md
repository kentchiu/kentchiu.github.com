---
author: Kent Chiu
published: true
layout: post
title: "時間日期處理"
date: 2012-09-30
comments: true
external-url:
sharing: true
footer: true
categories:
  - java
  - datetime
  - joda
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



日期的處理，在java中，一直是一個麻煩的問題，很多時間或日期操作若只是想透過JDK的[Date](http://download.oracle.com/javase/6/docs/api/java/util/Date.html "http://download.oracle.com/javase/6/docs/api/java/util/Date.html")
或[Calendar](http://download.oracle.com/javase/6/docs/api/java/util/Calendar.html "http://download.oracle.com/javase/6/docs/api/java/util/Calendar.html")來處理，會相當的不方便，況且
JDK內日期時間就有好幾種，很容易讓許多人無從下手，以下列出JDK
1.6內的日期時間相關物件(不含時間日期格式物件)

1.  [java.util.Date](http://download.oracle.com/javase/6/docs/api/java/util/Date.html "http://download.oracle.com/javase/6/docs/api/java/util/Date.html")
    java日期物件
2.  [java.sql.Date](http://download.oracle.com/javase/6/docs/api/java/sql/Date.html "http://download.oracle.com/javase/6/docs/api/java/sql/Date.html")
    資料庫用的日期物件
3.  [java.sql.Time](http://download.oracle.com/javase/6/docs/api/java/sql/Time.html "http://download.oracle.com/javase/6/docs/api/java/sql/Time.html")
    資料庫用的日期物件
4.  [java.sql.Timestamp](http://download.oracle.com/javase/6/docs/api/java/sql/Timestamp.html "http://download.oracle.com/javase/6/docs/api/java/sql/Timestamp.html")
    資料庫用的時間戳記物件
5.  [java.util.Calendar](http://download.oracle.com/javase/6/docs/api/java/util/Calendar.html "http://download.oracle.com/javase/6/docs/api/java/util/Calendar.html")
    java日期物件

目前處理日期相關的工具，除了上述的之外，JDK內還有[日期格式化的相關功能](http://wiki.kent-chiu.com/doku.php?id=java:date_and_time "java:date_and_time")，另外，還有
Apache
[connons-lang](http://commons.apache.org/lang/ "http://commons.apache.org/lang/")裡的[DateUtils](http://commons.apache.org/lang/api-2.6/org/apache/commons/lang/time/DateUtils.html "http://commons.apache.org/lang/api-2.6/org/apache/commons/lang/time/DateUtils.html")有許多好用的method可以做日期的判斷，
如果還是不足夠的話，還有時間日期處理的終極武器[Joda
time](http://joda-time.sourceforge.net/ "http://joda-time.sourceforge.net/")，可以很有效的對付時間日期區間的問題(Start
Time, End Time報表常會用到)， 那也可以看看[JSR
310時間日期處理功能](http://sourceforge.net/apps/mediawiki/threeten/index.php?title=ThreeTen "http://sourceforge.net/apps/mediawiki/threeten/index.php?title=ThreeTen")，這個原本有可能是[JDK
7](http://download.oracle.com/javase/7/docs/ "http://download.oracle.com/javase/7/docs/")的一部份，現在就不了了之了

### 日期時間常見的議題有

1.  例假日
2.  農民曆
3.  中華民國歷
4.  日光節約時間
5.  地區化
6.  格式化
7.  日期區間運算

Apache Commons Lang
===================

#### 時間的shift

-   addDays
-   addHours
-   addMinutes
-   …

#### 四捨五入，完全捨去，完全進位

-   ceiling
-   round
-   truncate

#### 其他

-   toCalendar(Date) Date轉Calendar
-   isSameDay 是否為同一天
-   parseDate(String, String[]) 將字串轉成日期
-   getFragmentInXXX() 經過了多少的時間單位
-   iterator

getFragmentInXXX()一系列的方法，可以計算出從何時到何時，總共經過了多時間，比如說，今年到今天，總共已經了多少天，或多少小時或多少秒，…

而iterator可以用以下的參數對某一個時間做iterator

1.  RANGE\_MONTH\_SUNDAY
2.  RANGE\_MONTH\_MONDAY
3.  RANGE\_WEEK\_SUNDAY
4.  RANGE\_WEEK\_MONDAY
5.  RANGE\_WEEK\_RELATIVE
6.  RANGE\_WEEK\_CENTER

Joda Time
=========

-   很容易從jdk的Date,Calendar轉換，只要傳入DateTime的Constructor即可
-   所有的datetime
    classes都是immutable，但有提供一些method可以方便的傳回運算後新的datetime

### 主要的觀念

-   Instant - 最基本的觀念，基本上就是一個時間點或DateTime
-   Partial -
-   Interval - 時間間隔，也就是start time 跟 end time
-   Period - 期間，像是3天，6個月，5個小時，可以從interval轉換過來
-   Duration -
    期間，跟Interval的不同是Duration是無timezone的，可以從interval轉換過來，但無法轉回interval
-   Chronology - 設計日曆的api用，大部份的情況下，可以不管它
-   DateTimeZone - Time Zone

Duration V.S Period

Duration是很簡純的觀念，它就是一個固定的時間(多少毫秒)，而Period則是會隨CONTEXT變化的時間單位，比如說Period如果為一個月，對
一月份一的個月，跟二月份的一個月，長度就不一樣。

ex: 1/15要加一個月的時間，用Period來處理，就是1 Month Period,
(記為P1M)，就是是2/15日，如果用Duration來處理，它只能固定為一個時間長度(換算成milliseconds)，1月到2月是(29天的換算成的milliseconds)，所以就無法用相同的一個Duration來計算下個月的15號時何時(2-3月是30天)。

base, chrono, convert, field and tz這幾個packages是private
package,一般的application大多字會用到`org.joda.time` package的內容

### 重要的Interface

##### ReadableInstant

1.  compareTo(Object)
2.  equals(Object)
3.  get(DateTimeFieldType)
4.  getChronology()
5.  getMillis()
6.  getZone()
7.  hashCode()
8.  isAfter(ReadableInstant)
9.  isBefore(ReadableInstant)
10. isEqual(ReadableInstant)
11. isSupported(DateTimeFieldType)
12. toInstant()
13. toString()

##### ReadableInterval

1.  contains(ReadableInstant)
2.  contains(ReadableInterval)
3.  equals(Object)
4.  getChronology()
5.  getEnd()
6.  getEndMillis()
7.  getStart()
8.  getStartMillis()
9.  hashCode()
10. isAfter(ReadableInstant)
11. isAfter(ReadableInterval)
12. isBefore(ReadableInstant)
13. isBefore(ReadableInterval)
14. overlaps(ReadableInterval)
15. toDuration()
16. toDurationMillis()
17. toInterval()
18. toMutableInterval()
19. toPeriod()
20. toPeriod(PeriodType)
21. toString()

##### ReadablePeriod

1.  equals(Object)
2.  get(DurationFieldType)
3.  getFieldType(int)
4.  getPeriodType()
5.  getValue(int)
6.  hashCode()
7.  isSupported(DurationFieldType)
8.  size()
9.  toMutablePeriod()
10. toPeriod()
11. toString()

joda-time中的interface跟一般framework中的interface不大一樣，一般的framework中，會建議儘量宣告物件成interface，但是在joda-time中，大多數的情況下，
必須用Concreate Class方能取得物件完整的能力

### ISO Date Format

[ISO\_8601](http://en.wikipedia.org/wiki/ISO_8601 "http://en.wikipedia.org/wiki/ISO_8601")是關於日期時間格式相關的ISO規定，裡面的日期，是用”-“做分隔符號(而不是”/”)，建議所有的日期格式與ISO
8601相容 這樣，很多lib(不管是前端或後端)都可以比較容易處理。

### code snippet



```
    Days days = Days.daysBetween(new DateTime(start), new DateTime(end));

```

parse含有AM/PM，要加上Locale.US，不然會出exception



```
    SimpleDateFormat df = new SimpleDateFormat("hh:mm aa", Locale.US);

```

如果有時確定格式是對的，但是就是一直出ParseException，這時記得加上Locale.US的參數試試



```
    import org.joda.time.DateTime;
    import org.joda.time.Interval;
     
    public enum Scheduling {
        DAILY, WEEKLY, MONTHLY,;
     
        public Interval interval(DateTime dt) {
            DateTime start;
            DateTime end;
            switch (this.name()) {
            case "DAILY":
                start = dt.withMillisOfDay(0);
                end = dt.withMillisOfDay(24 * 60 * 60 * 1000 -1);
                break;
            case "WEEKLY":
                start = dt.withDayOfWeek(1);
                end = dt.withDayOfWeek(7).withMillisOfDay(24 * 60 * 60 * 1000 -1);
                break;
            case "MONTHLY":
                start = dt.withDayOfMonth(1);
                int max = dt.dayOfMonth().getMaximumValue();
                end = dt.withDayOfMonth(max).withMillisOfDay(24 * 60 * 60 * 1000 -1);
                break;
            default:
                throw new UnsupportedOperationException("Unknow name :" + this.name());
            }
            return new Interval(start, end);
        }
     
    }
     
    //
    // test case 
    //
    import static org.hamcrest.Matchers.is;
    import static org.junit.Assert.assertThat;
     
    import java.util.Date;
    import java.util.Locale;
     
    import org.apache.commons.lang3.time.DateUtils;
    import org.joda.time.DateTime;
    import org.joda.time.Interval;
    import org.junit.Test;
     
    public class SchedulingTest {
     
        @Test
        public void interval_daily() throws Exception {
            Date date = DateUtils.parseDate("2012/02/14", "yyyy/MM/dd");
            Interval interval = Scheduling.DAILY.interval(new DateTime(date));
            assertThat("2012/02/14 00:00:00", is(interval.getStart().toString("yyyy/MM/dd HH:mm:ss")));
            assertThat("2012/02/14 23:59:59", is(interval.getEnd().toString("yyyy/MM/dd HH:mm:ss")));
        }
     
        @Test
        public void interval_weekly() throws Exception {
            Date date = DateUtils.parseDate("2012/02/14", "yyyy/MM/dd"); // 2012/2/14 was a Tuesday
            Interval interval = Scheduling.WEEKLY.interval(new DateTime(date));
            assertThat("2012/02/13 00:00:00", is(interval.getStart().toString("yyyy/MM/dd HH:mm:ss")));
            assertThat("2012/02/19 23:59:59", is(interval.getEnd().toString("yyyy/MM/dd HH:mm:ss")));
            assertThat(interval.getStart().dayOfWeek().getAsText(Locale.ENGLISH), is("Monday")); // which day is first day of week that depend on OS settings
            assertThat(interval.getEnd().dayOfWeek().getAsText(Locale.ENGLISH), is("Sunday"));  // which day is end day of week that depend on OS settings
        }
     
        @Test
        public void interval_monthly() throws Exception {
            Date date = DateUtils.parseDate("2012/02/14", "yyyy/MM/dd"); 
            Interval interval = Scheduling.MONTHLY.interval(new DateTime(date));
            assertThat("2012/02/01 00:00:00", is(interval.getStart().toString("yyyy/MM/dd HH:mm:ss")));
            assertThat("2012/02/29 23:59:59", is(interval.getEnd().toString("yyyy/MM/dd HH:mm:ss")));
        }
     
    }

```

Resources
=========

-   [http://commons.apache.org/lang/](http://commons.apache.org/lang/ "http://commons.apache.org/lang/")
    - Apache Commons Lang
-   [joda
    time](http://joda-time.sourceforge.net/ "http://joda-time.sourceforge.net/")
    - joda-time
-   [JSR
    310](http://sourceforge.net/apps/mediawiki/threeten/index.php?title=ThreeTen "http://sourceforge.net/apps/mediawiki/threeten/index.php?title=ThreeTen")
    - 採用了很多[joda
    time](http://joda-time.sourceforge.net/ "http://joda-time.sourceforge.net/")的API設計，但還沒穩定，建用還是用joda

