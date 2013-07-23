---
author: Kent Chiu
published: true
layout: post
title: "Effective Java Item List"
date: 2012-05-25
comments: true
external-url:
sharing: true
footer: true
categories:
  - java
  - best_practice
---




This item list come for [Effective Java Second Edition][1]

Creating and Destroying Objects
===============================

#### Item 1: Consider static factory methods instead of constructors

#### Item 2: Consider a builder when faced with many constructor parameters

#### Item 3: Enforce the singleton property with a private constructor or an enum type

#### Item 4: Enforce noninstantiability with a private constructor

#### Item 5: Avoid creating unnecessary objects

#### Item 6: Eliminate obsolete object references

#### Item 7: Avoid finalizers

Methods Common to All Objects
=============================

#### Item 8: Obey the general contract when overriding equals

#### Item 9: Always override hashCode when you override equals

#### Item 10: Always override toString

#### Item 11: Override clone judiciously

#### Item 12: Consider implementing Comparable

Classes and Interfaces
======================

#### Item 13: Minimize the accessibility of classes and members

#### Item 14: In public classes, use accessor methods, not public fields

#### Item 15: Minimize mutability

#### Item 16: Favor composition over inheritance

#### Item 17: Design and document for inheritance or else prohibit it

#### Item 18: Prefer interfaces to abstract classes

#### Item 19: Use interfaces only to define types

#### Item 20: Prefer class hierarchies to tagged classes

#### Item 21: Use function objects to represent strategies

#### Item 22: Favor static member classes over nonstatic

除非member classes有存取到outter class的nonstatic
method，不然，應該把member classes宣告為statics

Generics
========

#### Item 23: Don't use raw types in new code

#### Item 24: Eliminate unchecked warnings

#### Item 25: Prefer lists to arrays

#### Item 26: Favor generic types

#### Item 27: Favor generic methods


```
    // Generic method
    public static <E>Set<E>union(Set<E>s1, Set<E>s2) {
        Set<E>result = new HashSet<E>(s1);
        result.addAll(s2);
        return result;
    }
```

#### Item 28: Use bounded wildcards to increase API flexibility

#### Item 29: Consider typesafe heterogeneous containers


利用加上Class\<T\>來確定T的型別

```
    // Typesafe heterogeneous container pattern - API
    public class Favorites {
    public <T> void putFavorite(Class<T> type, T instance);
    public <T> T getFavorite(Class<T> type);
    }
```

Enums and Annotations
=====================

#### Item 30: Use enums instead of int constants

#### Item 31: Use instance fields instead of ordinals


#### Item 32: Use EnumSet instead of bit fields

如果要用位元開關的效果，可以考慮用EnumSet

#### Item 33: Use EnumMap instead of ordinal indexing

如果需要對Enum進行排序，可以用EnumMap

#### Item 34: Emulate extensible enums with interfaces

Enum是無法繼承的，如果需要類似的功能，可以考慮用interfaces


```
    // Emulated extensible enum using an interface
    public interface Operation {
        double apply(double x, double y);
    }
     
    public enum BasicOperation implements Operation {
        PLUS("+") {
            public double apply(double x, double y) {
                return x + y;
            }
        },
        MINUS("-") {
            public double apply(double x, double y) {
                return x - y;
            }
        },
        TIMES("*") {
            public double apply(double x, double y) {
                return x * y;
            }
        },
        DIVIDE("/") {
            public double apply(double x, double y) {
                return x / y;
            }
     
            private final String  symbol;
     
            BasicOperation(String symbol) {
                this.symbol = symbol;
            }
     
            @Override
            public String toString() {
                return symbol;
            }
        }
    };
     
    // Emulated extension enum
    public enum ExtendedOperation implements Operation {
        EXP("^") {
            public double apply(double x, double y) {
                return Math.pow(x, y);
            }
        },
        REMAINDER("%") {
            public double apply(double x, double y) {
                return x % y;
            }
        };
        private final String  symbol;
     
        ExtendedOperation(String symbol) {
            this.symbol = symbol;
        }
     
        @Override
        public String toString() {
            return symbol;
        }
    }
```

#### Item 35: Prefer annotations to naming patterns

只要想到要用method
name的rule做判斷邏輯時，可以考慮用annotations或許會更彈性

#### Item 36: Consistently use the Override annotation

#### Item 37: Use marker interfaces to define types

Methods
=======

#### Item 38: Check parameters for validity

#### Item 39: Make defensive copies when needed

#### Item 40: Design method signatures carefully

#### Item 41: Use overloading judiciously

#### Item 42: Use varargs judiciously

#### Item 43: Return empty arrays or collections, not nulls

#### Item 44: Write doc comments for all exposed API elements

General Programming
===================

#### Item 45: Minimize the scope of local variables

#### Item 46: Prefer for-each loops to traditional for loops

#### Item 47: Know and use the libraries

#### Item 48: Avoid float and double if exact answers are required

#### Item 49: Prefer primitive types to boxed primitives

#### Item 50: Avoid strings where other types are more appropriate

#### Item 51: Beware the performance of string concatenation

#### Item 52: Refer to objects by their interfaces

#### Item 53: Prefer interfaces to reflection

#### Item 54: Use native methods judiciously

#### Item 55: Optimize judiciously

#### Item 56: Adhere to generally accepted naming conventions

Exceptions
==========

#### Item 57: Use exceptions only for exceptional conditions

#### Item 58: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors

checked exceptions應該是來被rescue的，只有能被復原的狀況，才用check
exception

#### Item 59: Avoid unnecessary use of checked exceptions

#### Item 60: Favor the use of standard exceptions

#### Item 61: Throw exceptions appropriate to the abstraction

#### Item 62: Document all exceptions thrown by each method

#### Item 63: Include failure-capture information in detail messages

#### Item 64: Strive for failure atomicity

#### Item 65: Don't ignore exceptions

Concurrency
===========

#### Item 66: Synchronize access to shared mutable data

#### Item 67: Avoid excessive synchronization

#### Item 68: Prefer executors and tasks to threads

#### Item 69: Prefer concurrency utilities to wait and notify

#### Item 70: Document thread safety

#### Item 71: Use lazy initialization judiciously

#### Item 72: Don't depend on the thread scheduler

#### Item 73: Avoid thread groups

Serialization
=============

#### Item 74: Implement Serializable judiciously

#### Item 75: Consider using a custom serialized form

#### Item 76: Write readObject methods defensively

#### Item 77: For instance control, prefer enum types to readResolve

#### Item 78: Consider serialization proxies instead of serialized instances

[1]:http://java.sun.com/docs/books/effective/toc.html "http://java.sun.com/docs/books/effective/toc.html"