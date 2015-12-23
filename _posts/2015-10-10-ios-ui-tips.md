---
published: true
author: Kent Chiu
layout: post
title: "iOS UI Tips"
date: 2015-10-10 15:30
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


#### Unwind Segue

1. Unwind Segue 是提供一個從其他controller 跳回來的機制，如果要讓其他controller能跳回來，必需先提供一個 `@IBAction func unwindToFirstView(segue: UIStoryboardSegue) {}`
2. 從目前的controller 把  controller icon 拉到 exit，然後選擇剛剛在來源controller 宣告的action
3. 把目前的segue取名
4. 在目前的controller 找一個觸發點(通常是 button 的 touch event)，然後`controller.performSegueWithIdentifier`，名稱為剛剛取名的那個segue

> 需要特別注意的是第一個步驟，@IBAction那個function是定義在要跳回去的 controller，之後其他的動作都是在目前的controller實作   
> 第二個步驟可以選擇所有已宣告的 action，所以可以達到跳轉到位何一個 contrller 的效果


#### Without Storyboard
- 視覺化的部份可以用 playgroud 來 test run
- collection view的datasource如果不是在 controller裡conform，那datasource的建立要在controller外，不然datasource相關的methods不會被callback









