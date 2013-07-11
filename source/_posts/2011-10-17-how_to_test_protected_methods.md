---
author: Kent Chiu
published: true
layout: post
title: "How To Test Protected Methods"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - php
  - testing
---



In PHP5, the **protected** scope means only class itself and subclass
can access it. PHPUnit (3.x) is extends from
**\*PHPUnit\_Framework\_TestCase**\*, and PHP5 don't support
multi-inherited, therefore, there is not way to access test target
directly.

The only way I know that can test **protected** scope method. That is
used a adapter class that subclass the target class, and do re-publish
protected methods as public.

I don't how to test private method in PHP5. The mock object (stub , or
whatever )don't make sense in this case.

example
-------


```
    class Foo {
        protected function bar() {
            return "bar";
            }
    }
```


```
    class FooAdapter extends Foo {
            // re-publish tested method as public scope for test purpose only
        public function bar() {
            // calling parent tested method
                    return parent::bar();
        }
    }
     
    class FooTest extends PHPUnit_Framework_TestCase {
            // testing the subclass FooAdapter of Foo class instead.
        private $foo;
        public function testFoo() {
            $foo = new FooAdapter;
            // now we can invoke bar() method. 
            $foo.bar();
        }
    }
```

The Adapter Class should put in test folder with other testcase, and
never released as production.


