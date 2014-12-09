---
author: Kent Chiu
published: true
layout: post
title: "Class Type Hints"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - pdt
---




variable type hint by comment was deprecated after PDT 2.1

By using a comment you can assign a variable its exact class value. This
assignment will affect the code assist of this variable accordingly.



```
    <?php 
    function getClass() { 
    return new Test(); 
    } 
    class Test { 
    function printValues($a, $b) { 
    echo "Values: $a, $b"; 
    } 
    } 
    $myVar = getClass(); 
    /* @var $myVar Test */ 
    $myVar-> 
    ?> 

```



```
    <?php 
    Class MyClass {
      /**
       * @var ClassType
       */ 
      protected $_foo;
    }
    ?> 

```

Place your cursor after '\$myVar→' (on the line above the closing PHP
tag) and press Ctrl+Space to activate Code Assist. Code Assist will open
with the function defined in 'Test' class (printValues(\$a, \$b)).
Double click it to enter it into your code.

Without the comment, Code Assist will not be available for the function.

you can also add this to templates



```
    /* @var ${dollar}${variable} ${class} */

```



```
    /**
     * @var ClassType
     */ 

```

check
[here](http://www.alexatnet.com/node/179 "http://www.alexatnet.com/node/179")
for more information.

