---
author: Kent Chiu
published: true
layout: post
title: "ASCII UML Class Diagram"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - programming
---






```
     Association:   ^   Aggregation:    O   Composition:    @   Inheritance:    #
                    |                   |                   |                   |
                  <-+->               O-+-O               @-+-@               #-+-#
                    |                   |                   |                   |
                    V                   O                   @                   #

```



```
     +--------+       +---------------------+ *
     | Client |------>| Component           |<-----------------------------+
     +--------+       +---------------------+                              |
                      | Operation()         |                              |
                      | Add(Component)      |                              |
                      | Remove(Component)   |                              |
                      | GetChild(int)       |                              |
                      +---------------------+                              |
                                #                                          |
                                |                                          |
                     +----------+--------------+                           |
                     |                         |                           |
              +------+------+       +----------+----------+  children      |
              | Leaf        |       | Composite           |O---------------+
              +-------------+       +---------------------+
              | Operation() |       | Operation()         |
              +-------------+       | Add(Component)      |
                                    | Remove(Component)   |
                                    | GetChild(int)       |
                                    +---------------------+

```



```
      +-------+         +-----------+
      | Base  |         | Interface |
      +---^---+         +-----^-----+
         /_\                 /_\     _
          |                   :     (_) OtherInterface
          |                   :      |
          |                   :      |
     +---------+      +----------------+
     | Derived |      | Implementation |
     +---------+      +----------------+

```



```
    +------------------------------------------------------+
    |                                                      |  
    +------------------------------------------------------+
    |                                                      |
    |                                                      |
    |                                                      |
    |                                                      |
    +------------------------------------------------------+
    |                                                      |
    |                                                      |
    |                                                      |
    +------------------------------------------------------+

```

Resources
=========


