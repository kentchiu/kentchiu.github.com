---
author: Kent Chiu
published: true
layout: post
title: "Smalltalk Best Practice Patterns閱讀筆記"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - programming
---



Smalltalk Best Practice Patterns是Kent
Beck的經典之著，書名看起來是Smalltalk語言的最佳實踐，但一樣適合在其他語言。本書承習著K.B的寫書的特色，輕薄短小，字字珠磯。以下是我一邊閱讀時一邊紀錄下的筆記，我這本書沒有很細仔去看，但是大約的掠過，所以可能對內容上會有誤解。

程式風格 (Style)
----------------

這邊的風格不是只是狹義的程式風格慣例(convention)，而是一些程式撰寫時的Style。程式好的風格如下：

-   Once and only once - 不要有重覆的程式碼
-   Lots of little pieces - 短一點的method
-   Replacing objects - a.k.a code for interface
-   Moving objects - Another property of systems with good style is that
    their objects can be easily moved to new contexts
-   Rates of change - don’t put two rates of change together

