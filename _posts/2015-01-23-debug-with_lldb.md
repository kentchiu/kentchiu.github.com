---
published: true
author: Kent Chiu
layout: post
title: "利用LLDB debugging SWFIT 程式"
date: 2015-01-23 12:03
comments: true
sharing: true
footer: true
tags: 
- ios
- xcode
- debugging
- lldb
---

進入LLDB有兩種方式:

1. 一種是在xcode使用 debugger，程式停在breakpoint 好, debug console會有一個 `(lldb) `的提示符
2. 另一種是在 Termianl 中執行 `xcrun swift` 

> 在 lldb 跟 REPL 兩種模式切換的方式如下:
> lldb -> repl : 使用 `repl` 指令
> repl -> lldb : 使用 `:` 指令
> 
> REPL : 程式未執行下，用來評估各種expression
> lldb : 探偵執行中程式的行為

LLDB 常用指令:

- thread info 
- thread backtrace (bt) 
- frame select # (f #)
- expression (p)


# Resources 
- The LLDB Debugger <http://lldb.llvm.org/lldb-gdb.html>




