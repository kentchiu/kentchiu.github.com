---
published: true
author: Kent Chiu
layout: post
title: "IOS開發如何套用template"
date: 2015-03-12 09:11
tags: 
- ios
- xcode
- ui
---

網路上常常可以看到一些很漂亮的ui，也看到許多人的做出來的app ui都不像官方版的預設的那樣，或者是，收到美工給的prototype，用色跟外觀通常也不見得用storyboard拉一拉就可以做得出來。

目前看到主要的解法有兩種:

1. 大部份的畫面直接用拿到的prototype的畫面background，然後，把會動的部份換成透明的元件
2. 如果ui元件的操作比較特別，那就要客製元件

第一種方式比較簡單，也可以解決掉大部份的問題，第二種方式，需要花比較多功夫，但做完後的元件可以reusing，可以有比較多特殊的效果

後來看了 [design+code](https://designcode.io/) 的書，對許多ui的製作已經有很好的了解，雖然這本書的主要對象是 ui designer，但我覺得對ios programming也有很大的幫助
結果也順趨的學了 [Sketch](http://bohemiancoding.com/sketch/), 之前花過數周的時間學 photoshop 跟 illustrator，都因成效不彰而放棄，
但照著 design+code 的教學，目前也能還算熟練著操作sketch了