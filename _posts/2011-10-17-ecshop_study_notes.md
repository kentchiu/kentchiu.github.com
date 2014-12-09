---
author: Kent Chiu
published: true
layout: post
title: "ECShop Study Notes"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ecshop
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




ECShop provides themes changed capability, it mean we can switch themes
to change the look and feels of web site(front end only). The capability
come from the MVC pattern, it is abbreviated
**M**odel-**V**iew-**C**ontroller.

#### Model

In ECShop, the model is not mapping to code level (it means there is no
User object mapping to user table). But we can treating database like
the model.

the detail about the model of ECShop, please check [ECShop Database
Schema](http://wiki.kent-chiu.com/doku.php?id=ecshop:database_schema "ecshop:database_schema")

#### View

A theme is all about presentation, all files in the themes
folder(\*.dwt, \*.lbi, \*.js, \*.css) might very difference with other
themes.

the detail about the model of ECShop, please check
[這篇文章說明每個模版的功能](http://wiki.kent-chiu.com/doku.php?id=ecshop:template_index "ecshop:template_index")

#### Controller

The controller part (php files under root folder) that is most important
part of MVC, when we switch between this theme and that theme, the
controller would not changed, we only need to know which data is push to
view in controller. Whatever theme is using, it would not out of scope
of the data that pushed by controller.

Regarding the point of view, the most important thing to study ECShop is
to learn what controllers done.

the detail about the model of ECShop, please check
[Controllers](http://wiki.kent-chiu.com/doku.php?id=ecshop:controller_pages "ecshop:controller_pages")

debug
-----

1.  using IDE's debug as → PHP script
2.  using IDE's debug as → PHP web page
3.  using this code snippet to output variables to file
    file\_put\_contents(“debug.log”, var\_export(\$var, true));
4.  ecshop modify the smarty source, so that, we can **NOT** using
    smarty debuging console.

Invoke Sequence
---------------

ex: index page


```
http://www.domain.com/index.php -> ECSHOP_HOME/index.php -> index.dwt -> some of lbi files -> render to html.

```

(TBD: rewrite this more clearly)

Smarty
------

-   Initialized at includes/init.php
-   ECShop overwrites smarty as cls\_template, therefore, a part of
    smarty function maybe not provides in ECShop.
-   ECShop uses **\*\<!–{ }–\>**\* to make template file more friendly
    to WYSIWYG editor.

the **\#BeginLibraryItem** is not come from Smarty, it is come from
Dreamweaver, it means include the library item from
/library/auction.lbi. the library item is nothing more than a html file.


```
<!-- #BeginLibraryItem "/library/auction.lbi" --> 

```

includes/init.php
-----------------

-   every page should page **\*define('IN\_ECS', true);**\* or get
    Hacking attempt
-   database resource(cls\_mysql instant) is put into \$GLOBALS['db']

### Session Control


```
@ini_set('memory_limit',          '64M');
@ini_set('session.cache_expire',  180);
@ini_set('session.use_trans_sid', 0);
@ini_set('session.use_cookies',   1);
@ini_set('session.auto_start',    0);

```

### Init User Session


```
if (!defined('INIT_NO_USERS'))
{
    /* 初始化session */
    include(ROOT_PATH . 'includes/cls_session.php');
 
    $sess = new cls_session($db, $ecs->table('sessions'), $ecs->table('sessions_data'));
 
    define('SESS_ID', $sess->get_session_id());
}

```

### Display Error

@ini\_set('display\_errors', 1);

### DEBUG MODE

debug mode using bit operation to determine which mode is in using , it
means if DEBUG MODE = 15(binary is 1111), it also includes DEBUG MODE =
1,2,4, 8, if DEBUG MODE = 7 (binary 0111), it is also includes DEBUG
MODE = 1,2,4

  code   description
  ------ ----------------------
  0      disabled debug
  1      output error message
  2      disabled caching
  4      showing debug page
  8      logging SQL query

#### code 0

-   disabled debug

#### code 2

##### smarty

-   force complied
-   direct output

##### includes/lib\_base

-   disabled cache writing
-   disabled cache reading.

#### code 4

-   showing debug page (includes/lib.debug.php)

#### code 8

-   logging SQL query statement to data/mysql\_query\_xxxxx

### User Input Checking


```
/* 对用户传入的变量进行转义操作。*/
if (!get_magic_quotes_gpc())
{
    if (!empty($_GET))
    {
        $_GET  = addslashes_deep($_GET);
    }
    if (!empty($_POST))
    {
        $_POST = addslashes_deep($_POST);
    }
 
    $_COOKIE   = addslashes_deep($_COOKIE);
    $_REQUEST  = addslashes_deep($_REQUEST);
}

```

includes/cls\_mysql.php
-----------------------

-   all database operations should using this class
-   this class has cache mechanism
-   if you don't want to cache a table, you can
    set\_disable\_cache\_tables() method

amdin/shop\_config.php
----------------------

-   the main config file of ECShop
-   this file **would be caching** for performance reason, if you want
    to change this file, need refresh cache.

regarding the caching, the we can find the compiled file
shop\_config.php in temp/static folder, it contains configuration
information, it's useful for debug.

AJAX
----

ecshop裡一ajax主要是透過transport.js裡的AJAX.call.


```
  /* *
  * 调用此方法发送HTTP请求。
  *
  * @public
  * @param   {string}    url             请求的URL地址
  * @param   {mix}       params          发送参数
  * @param   {Function}  callback        回调函数
  * @param   {string}    ransferMode     请求的方式，有"GET"和"POST"两种
  * @param   {string}    responseType    响应类型，有"JSON"、"XML"和"TEXT"三种
  * @param   {boolean}   asyn            是否异步请求的方式
  * @param   {boolean}   quiet           是否安静模式请求
  */
  call : function (url, params, callback, transferMode, responseType, asyn, quiet)
  // using : Ajax.call( 'user.php?act=is_registered', 'username=' + username, registed_callback , 'GET', 'TEXT', true, true );

```

