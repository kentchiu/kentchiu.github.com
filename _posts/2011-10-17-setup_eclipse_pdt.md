---
author: Kent Chiu
published: true
layout: post
title: "How to setup PDT"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - pdt
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------


PDT is Eclipse **P**HP **D**evelopment **T**ools, this article is a
instuction about how to setup PDT with other useful tools, like:

1.  zend debugger
2.  remote server

Option:

1.  SVN plugins
2.  open extern plugins (open contain folder or shell) ()
3.  QuickREx (Regular Expression tools)

Install PDT by pulse
====================

1.  download
    [pulse](http://www.poweredbypulse.com/ "http://www.poweredbypulse.com/")
2.  registry a account if necessary
3.  install Eclipse 3.x For PHP in pulse menu

Install Plugins (Optional)
==========================

searching Subversive, open extern and QucikREx as keyword in pules
Browser Categories node then install.

Install Debugger
================

there are two debugger supports by PDT now

1.  zend debugger
2.  XDebug

both two can used as Script Debugger or Server Debugger of PDT debug
functions.

zend debugger install
---------------------

Assumption: PHP install at C:\\xampp\\php

### downloading

Download zend debugger extention from
[here](http://downloads.zend.com/pdt/server-debugger "http://downloads.zend.com/pdt/server-debugger"),
we use
[ZendDebugger-5.2.15-cygwin\_nt-i386.zip](http://downloads.zend.com/pdt/server-debugger/ZendDebugger-5.2.15-cygwin_nt-i386.zip "http://downloads.zend.com/pdt/server-debugger/ZendDebugger-5.2.15-cygwin_nt-i386.zip")
in this case(maybe you can find the new version then this).

### copy dll to php ext folder

copy ZendDebugger.dll to C:\\xampp\\php\\ext folder it maybe not as same
as your path, you need found out somewhere you installed the php.

### put dummy.php to your web root

saving follow code as dummy.php and put it to web root(e.g
c:\\xampp\\htdocs\\dummy.php)



```
    <?php
    @ini_set('zend_monitor.enable', 0);
    if(@function_exists('output_cache_disable')) {
        @output_cache_disable();
    }
    if(isset($_GET['debugger_connect']) && $_GET['debugger_connect'] == 1) {
        if(function_exists('debugger_connect'))  {
            debugger_connect();
            exit();
        } else {
            echo "No connector is installed.";
        }
    }
    ?>

```

### setup php.ini

put this section to your php.ini.



```
    ; this is to see output while debugging
    implicit_flush = On 
     
    ; this is to see output while debugging
    output_buffering = 0
     
    [Zend]
    zend_extension_ts="C:\xampp\php\ext\ZendDebugger.dll"
    zend_debugger.allow_hosts=127.0.0.1/32
    zend_debugger.expose_remotely=always

```

XDebug install
--------------

### downloading

Download zend debugger extention from
[[http://www.xdebug.org/download.php](http://www.xdebug.org/download.php "http://www.xdebug.org/download.php")|here]].

### copy dll to php ext folder

copy php\_xdebug\_xxx.dll to C:\\xampp\\php\\ext folder it maybe not as
same as your path, you need found out somewhere you installed the php.

### setup php.ini

put this section to your php.ini.



```
    [XDebug]
    ;; Only Zend OR (!) XDebug
    zend_extension_ts="C:\xampp\php\ext\php_xdebug.dll"
    xdebug.remote_enable=true
    xdebug.remote_host=127.0.0.1
    xdebug.remote_port=9000
    xdebug.remote_handler=dbgp
    xdebug.profiler_enable=1
    xdebug.profiler_output_dir="C:\xampp\tmp"

```

PDT setup
=========

setup php executable
--------------------

setup php manual
----------------

point to here:
[http://www.php.net/manual/en](http://www.php.net/manual/en "http://www.php.net/manual/en")

set file association
--------------------

### HTML editor

eclipse menu → window → preference

1.  type 'content types' in filter filed
2.  select 'HTML' node
3.  click 'Add…' button to pop-up Add Content Type Association window
4.  input '\*.dwt' in Content type field
5.  click 'OK' to close Add Content Type Association window
6.  click preference window

![setup_elcipse_pdt_01.png][setup_elcipse_pdt_01.png]

### Web Page Editor

eclipse menu → window → preference

1.  type 'File Accociation' in filter filed
2.  Selecte **File Accociation** node
3.  select **Add…** Button to pop-up **Add File Type** window
4.  Typing which file type you want to add (should start with \*.)
5.  select **Add…** Button to pop-up
    **![setup_elcipse_pdt_02.png][setup_elcipse_pdt_02.png]
    ![setup_elcipse_pdt_03.png][setup_elcipse_pdt_03.png]
    - select the file type you want to associated to editor. -
    select**Add…**Button to pop-up**Editor Selection**window -
    click**OK**Button and confirm the file type is showed in**File
    Association\*\* List.

![setup_elcipse_pdt_04.png][setup_elcipse_pdt_04.png]

Web Page Editor introduced in WTP 2.0, it provides design time preview
functionally.

![](http://www.eclipse.org/webtools/releases/2.0/newandnoteworthy/j2ee/jsf-web-page-editor.png)

### Internet Web Browser

Internet Web Browser can used to preview HTML render with in Eclipse
View Site.

![](http://www.eclipse.org/webtools/initial-contribution/IBM/evalGuides/ServerToolsEval_files/monitorBrowser.gif)

We also can set \*.dwt, \*.lbi, \*.htm, \*.html file association to
those editors.

Verifing
========

-   create new project, the project location should set to
    WWW\_DOC\_ROOT\\projectName
-   create php file
-   run with script or server should all works fine.

Resoures
========

-   [pulse](http://http://www.poweredbypulse.com/ "http://http://www.poweredbypulse.com/")
    home
-   there are some usful information about how to configurated in [pdt
    wiki](http://www.thierryb.net/pdtwiki/index.php?title=Main_Page "http://www.thierryb.net/pdtwiki/index.php?title=Main_Page")
-   the details of how to setup zend debug can be found
    [here](http://www.thierryb.net/pdtwiki/index.php?title=Using_PDT_:_Installation_:_Installing_the_Zend_Debugger "http://www.thierryb.net/pdtwiki/index.php?title=Using_PDT_:_Installation_:_Installing_the_Zend_Debugger")
-   the deatils of how to using script and server debugger of PDT,
    please check this
    [reference](http://www.thierryb.net/pdtwiki/index.php?title=Using_PDT_:_User_Guide_:_PHP_Source_Level_Debugging "http://www.thierryb.net/pdtwiki/index.php?title=Using_PDT_:_User_Guide_:_PHP_Source_Level_Debugging")


[setup_elcipse_pdt_01.png]: http://blog.kent-chiu.com/images/2011-10-17/setup_elcipse_pdt_01.png
[setup_elcipse_pdt_02.png]: http://blog.kent-chiu.com/images/2011-10-17/setup_elcipse_pdt_02.png
[setup_elcipse_pdt_03.png]: http://blog.kent-chiu.com/images/2011-10-17/setup_elcipse_pdt_03.png
[setup_elcipse_pdt_04.png]: http://blog.kent-chiu.com/images/2011-10-17/setup_elcipse_pdt_04.png
