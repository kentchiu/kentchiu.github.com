---
author: Kent Chiu
published: true
layout: post
title: "SimpleXML"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - php
  - xml
---




-   access single element by name
-   access multi elements by array with name



```
    <members>
        <user id="kent">Kent Chiu</user>
        <user id="leon">Leon Wong</user>
    </members>

```



```
      $members = simplexml_load_file('member.xml'); // root
        $root->user[0]; // Kent Chiu
        $root->user[1]; // Leon Wong
            $root->user[0][id]; // kent
        $root->user[0] = 'DraculaCwg'; // now Kent Chiu is replaced to DraculaCwg
        $root->user[2] = "New User"; // appends a node 
        $root->user[2][id] = "new";

```

Resources
---------

-   [SimpleXML at
    PHP.net](http://tw2.php.net/manual/en/book.simplexml.php "http://tw2.php.net/manual/en/book.simplexml.php")


