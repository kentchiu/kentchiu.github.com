---
author: Kent Chiu
published: true
layout: post
title: "find Command"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - linux
---





Examples


```
find -name 'mypage.htm'

```

In the above command the system would search for any file named
mypage.htm in the current directory and any subdirectory.


```
find / -name 'mypage.htm'

```

In the above example the system would search for any file named
mypage.htm on the root and all subdirectories from the root.


```
find -name 'file*'

```

In the above example the system would search for any file beginning with
file in the current directory and any subdirectory.


```
find -name '*' -size +1000k

```

In the above example the system would search for any file that is larger
then 1000k.


```
find . -size +500000 -print

```

Next, similar to the above example, just formatted differently this
command would find anything above 500MB.

