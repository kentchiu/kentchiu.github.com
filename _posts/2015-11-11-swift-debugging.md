---
published: true
author: Kent Chiu
layout: post
title: "Swift Debugging"
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



四個可以客製 debug 格式的 protocols
- CustomStringConvertible
- CustomDebugStringConvertible 
- CustomPlaygroundQuickLookable 
- CustomReflectable








