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


LLDB 在 debug mode 下才會生效

help 

`help` 可以看到指令說明

Type Lookup

```bash
(lldb) type lookup ErrorType
protocol ErrorType {
  var _domain: Swift.String { get }
  var _code: Swift.Int { get }
}
extension ErrorType {
  var _domain: Swift.String {
    get {}
  }
}
```

examining data

- frame variaiable (fr v)
- expression -O (po)
- expression (p)
 
 frame varialbe 可以印出local varialbe

```bash
(lldb) frame variable
(Int, String) myTuple = (0 = 12, 1 = “Hello World”)
(Int) theYear = 1984

(lldb) frame variable theYear (Int) theYear = 1984
frame variable

(lldb) fr v
(Int, String) myTuple = (0 = 12, 1 = “Hello World”)
(Int) theYear = 1984
```

expression p

TBD : 多行模式的使用方式

```swift
(lldb) p print("hello swift!!")
hello swift!!
```
執行後的結果 $R1 可以再來拿使用

```swift
(Int) theYear = 1984
(lldb) p theYear + 1
(Int) $R1 = 1985
(lldb) p $R1 + 2
(Int) $R2 = 1987
```



# Resources 
- The LLDB Debugger <http://lldb.llvm.org/lldb-gdb.html>




