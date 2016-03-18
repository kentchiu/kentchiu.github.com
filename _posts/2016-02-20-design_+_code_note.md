---
published: false
author: Kent Chiu
layout: post
title: "Design+Code Note"
date: 2016-02-20 14:30
tags:
- sketch
---

## Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


----------------------------------------------------------------

## Design From Scratch

* 先選1個主色(明亮色) + 一組對比色(黑 vs 白)，主色用在 link，button等，對比色一個用在大部份的內容，另一個用在其他主色外的其他地方
* 空白 + 內容(含圖片)應佔畫面的80~90%
* 設計時選建議使用 iphone artborad 用 @1x 的size
* 利用Shadow, Gradient及Opacity增加可讀性及色彩的豐富性
* 行距建議為字體高度的 1.2 ~ 1.5倍
* 邊距是20 跟 8 (ios ui設計指南)
* 利用 rulers 對齊
* 可以把主色設成default style，在 menu > edit > Set Style As Default

## Design From Screen
* 所有Shadow 的 position (x, y)應該要一致，會比較協調
* 可利用兩個Shadow來讓畫面更有層次感
  1. internal shadow 用來繪制邊框
  2. drop shadow 用來繪制陰影
  3. shadow 的 opacity 設定少一點可以讓設計不致於太過"搶眼"
* cmd + shift + v to paste to layer

## Build An App
* TableViewCell 不要用`textLabel`跟`imageView`當元件名稱，應該`UITablewCell`裡已經有定義這兩個元件了
* Assistant Editor的Automatic mode 無法選到 Cell View的Source Fie時，可以把focus放在Cell View下的Content View,即可在breadcrumb bar選到Cell View的Source File

tableView要在`viewDidLoad()`加上這段才會重算高度，不然TableViewCell會是44 pt的高度
```
tableView.estimatedRowHeight = 100
tableView.rowHeight = UITableViewAutomaticDimension
```
