---
published: true
author: Kent Chiu
layout: post
title: "IOS auto layout 筆記"
date: 2015-02-02 09:11
tags: 
- ios
- xcode
---

- 部份元件會自動推算大小(intrinsic size), 所以在設定大小可以不用設定大小的 constrain
- 儘量用相對與其他 view 的方式決定位置, 然後由元件的 intrinsic size 決定大小
- 橘色的"T-bar" 表示 constrains 設定把完整,無法計算出 view 的位置及大小,需要再加入其他的 constrain (missing constrains),讓"T-bar"直至成為藍色為止
- 設定多個 constrains 的方式是按住 constrain 後多選數個 views 加入 constrain
- 部份元件,在沒有加入任何 constrain 前, layout 不會跑掉是因為 auto layout constrain 
- 先處理 sub view 間的 constrains 關係,讓它 "群組化"後再設定與 super view 會比較容易

