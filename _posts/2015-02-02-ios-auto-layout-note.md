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

- 部份元件會自動推算大小(intrinsic size), 所以在設定大小可以不用設定大小的 constraint
- 儘量用相對與其他 view 的方式決定位置, 然後由元件的 intrinsic size 決定大小
- 橘色的"T-bar" 表示 constraints 設定把完整,無法計算出 view 的位置及大小,需要再加入其他的 constraint (missing constraints),讓"T-bar"直至成為藍色為止
- 設定多個 constraints 的方式是按住 constraint 後多選數個 views 加入 constraint
- 部份元件,在沒有加入任何 constraint 前, layout 不會跑掉是因為 auto layout constraint 
- 先處理 sub view 間的 constraints 關係,讓它 "群組化"後再設定與 super view 會比較容易
- 如果要設定subview小於super view的一定比例以下,可在subview 與 super view 間拉 *Equal Heights Constraint* 或 *Equal Widths Constraint*, 然後設定 *Relation* 為 *Less Than or Equal* (可能需先對調 *First Item*, *Second Item*)
- 先弄出可以適用於所有 devices 的通用 layout, 在針對特定的layout調整, 如果能不用到 size class 就不要用
- font 的設定是全局的, 一旦設定就會影響到 base layout，如果要為特定的layout 設定專用的font, 必須為該layout新增一特定的font
- constraint 跟其他的 UI 元件一樣,也是可以透過 `@IBOutelet` 用程式控制
