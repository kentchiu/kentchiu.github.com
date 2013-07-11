---
author: Kent Chiu
published: true
layout: post
title: "Watching Queries Executing"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - mysql
---




Enabled slow queries will hit performance

Enabled Slow Queries
====================

check log status

```
mysql>show variables like 'log_%'; 
```

The config file should be named my.ini or my.cnf.

In Linux, file would be located at /etc; in Windows would be locate in
mysql\_home\\bin

To enable slow queries, put this section in your config file

```
[mysqld]
# log-error="mysql/logs/error.log"  
# log="mysql/logs/mysql.log"
# logging query every 2 seconds    
#if query is more then long_query_time seconds, it will be logging.  
# log-slow-queries= "mysql/logs/slowquery.log"  
```

\<note note\> You need to restart mysql after configured. \<note\>

