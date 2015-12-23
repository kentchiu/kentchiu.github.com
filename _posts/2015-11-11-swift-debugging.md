---
published: true
author: Kent Chiu
layout: post
title: "Swfit Debugging"
date: 2015-11-11 14:30
tags: 
- debug
- lldb
- llvm
- swift
---

## Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


----------------------------------------------------------------



## Undocument methos for debugging

這些api都是 obj c的 private method，在 swift環境下是沒法直接用的，這邊有[解法](https://medium.com/ios-os-x-development/auto-layout-debugging-in-swift-93bcd21a4abf)

- `_recursiveDescription` (or `recursiveDescription`)
- `_autolayoutTrace`
- `-_ivarDescription`
- `-_methodDescription`


## LLDB

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


四個可以客製 debug 格式的 protocols
- CustomStringConvertible
- CustomDebugStringConvertible 
- CustomPlaygroundQuickLookable 
- CustomReflectable








