---
author: Kent Chiu
published: true
layout: post
title: "如何讀取許多小icons組合而成圖檔"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - web
  - tips
---



設計網站時，常會用到許多小小的icon檔(任何圖檔格式)，如果每個小小的icon都是獨立的檔案，這樣會比較不好管理。

解決的方式是先將數個小圖檔給組合成一個單一的圖檔，然後用css定位跟指定大小的方式來顯示需要的icon。

![icons_combined.png][icons_combined.png]

以上面的圖檔來說，我們若只需要x這個icons，所以我們可以用以下的css語法來顯示這個icon

```
x-icon {
  background-position: -96px -128px; // icons的位置 
  width: 16px;                       // icons的大小
  height: 16px;                      // icons的大小 
}
```



[icons_combined.png]: http://blog.kent-chiu.com/images/2011-10-17/icons_combined.png
