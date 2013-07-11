---
author: Kent Chiu
published: true
layout: post
title: "Running PHPUnit in Eclipse"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - pdt
  - testing
---




This article is demonstrate how to run PHPUnit test cases in Eclipse
PDT.

There is no build in support for PHPUnit in PDT 2.0 now (however it
supports by Zend Studio). If we want to run the PHPUnit test cases, we
need some additional works for it.

We can invoked phpunit.bat that includes in PHPUnit package by eclipse
external tools function.

this configuration can be reused in any PHPUnit test case. you don't
need to configuring it in every test cases, as long as you need to
select the test case^[1)](#fn__1)^ first then run it. .

STEPS
-----

1.  [add PHPUnit supports to
    Eclipse](#add_phpunit_supports_to_eclipse "php:running_phpunit_in_eclipse ↵")
2.  [write a simple
    test](#write_a_simple_test "php:running_phpunit_in_eclipse ↵")
3.  [run the test
    case](#run_the_test_case "php:running_phpunit_in_eclipse ↵")
4.  [check resuls](#check_resuls "php:running_phpunit_in_eclipse ↵")

### add PHPUnit supports to Eclipse

select eclipse main menu → window → preference

1.  select **PHP Include Path** node
2.  select **Libraries** tab
3.  click **Add External Source Folder …**
4.  select where your PEAR installed
5.  click **OK** button to close folder selection window.
6.  click **OK** to complete setting.

![running_phpunit_in_eclipse_01.png][running_phpunit_in_eclipse_01.png]

### write a simple test


```
      <?php
        require_once 'PHPUnit/Framework.php';
     
        class ArrayTest extends PHPUnit_Framework_TestCase {
            public function testNewArrayIsEmpty() {
                $array = array();
                $this->assertEquals(0, sizeOf($array));
            }
     
            public function testArrayContainsAnElement() {
                $array[] = "content";
                $this->assertEquals(1, sizeof($array));
            }
     
        }
     
        ?>
```

### run the test case as Debug

copy file content from %PHP\_HOME%\\phpunit and paste as
phpunit\_runner.php^[2)](#fn__2)^


```
    <?php
    if (strpos('C:\Program Files\VertrigoServ\Php\php.exe', '@php_bin') === 0) {
        set_include_path(dirname(__FILE__) . PATH_SEPARATOR . get_include_path());
    }
     
    require_once 'PHPUnit/Util/Filter.php';
     
    PHPUnit_Util_Filter::addFileToFilter(__FILE__, 'PHPUNIT');
     
    require 'PHPUnit/TextUI/Command.php';
     
    define('PHPUnit_MAIN_METHOD', 'PHPUnit_TextUI_Command::main');
     
    PHPUnit_TextUI_Command::main();
    ?>
```

setup:

1.  selected phpunit\_runner.php in PHP project explorer view.
2.  click eclipse main menu → Debug Configurations… to poped-up **Debug
    Configurations** window
3.  new a PHP script
4.  type 'phpunit\_runner' in Name field.
5.  make sure PHP file field is selected to phpunit\_runner.php file
6.  switch to **PHP Script arguments** tab
7.  type project absolute path in Script Arguments field

![running_phpunit_in_eclipse_04.png][running_phpunit_in_eclipse_04.png]

![running_phpunit_in_eclipse_05.png][running_phpunit_in_eclipse_05.png]

debug:

1.  mark any breakpoint in test case.
2.  select a test case file (php file) which you want debug.
3.  click drop-list of **Debug** button in toolbar
4.  click **phpunit\_runner** which we created before then it will
    invoke PHP script Debug.

![running_phpunit_in_eclipse_06.png][running_phpunit_in_eclipse_06.png]

please make sure you selected the test case file before invoke
phpunit\_runner

### run the test case as External Tool

1.  click eclipse main menu → Run → External Tools → External Tools
    Configurations… to poped-up **External Tools Configurations** window
2.  select \*Main\* tab
3.  click **Browse File System…** to fulfill ^[3)](#fn__3)^ **Location**
    field
4.  click **Browse Workspace…** to fulfill **Working Directory** field.
5.  click **Variables…** to fulfill **Arguments** field.
6.  click **Run** button to run PHPUnit test cases.

![running_phpunit_in_eclipse_02.png][running_phpunit_in_eclipse_02.png]

### check resuls

the test results should be showed in **Console** view.

![running_phpunit_in_eclipse_03.png][running_phpunit_in_eclipse_03.png]




^[1)](#fnt__1)^ any php file

^[2)](#fnt__2)^ C:\\Program Files\\VertrigoServ\\Php is my %PHP\_HOME%
in case

^[3)](#fnt__3)^ phpunit.bat was gernerated after PHPUnit installed
[running_phpunit_in_eclipse_01.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_01.png
[running_phpunit_in_eclipse_04.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_04.png
[running_phpunit_in_eclipse_05.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_05.png
[running_phpunit_in_eclipse_06.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_06.png
[running_phpunit_in_eclipse_02.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_02.png
[running_phpunit_in_eclipse_03.png]: http://blog.kent-chiu.com/images/2011-10-17/running_phpunit_in_eclipse_03.png
