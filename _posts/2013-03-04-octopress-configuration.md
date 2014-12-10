---
author: Kent Chiu
published: true
layout: post
title: "Octopress配置"
date: 2013-03-04 00:31
comments: true
sharing: true
footer: true
tags:
  - octopress 
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




配置檔說明在 : https://github.com/mojombo/jekyll/wiki/Configuration

plugins : https://github.com/imathis/octopress/wiki/3rd-party-plugins

小抄: http://dreamrunner.org/wiki/public_html/docs/Web/octopress.html

---

#### 加快generate的速度

文章數不到200篇，但在i7的cpu上generate已經要需近一分鐘的時間，還好octopress提供了一個快速generate的指令`rake isolate`
，動作原理就是開一個暫存的目錄`_stash`要generate前會把`_post`裡的所有檔案copy到暫存的目錄`_stash`下，等要
deploy時，再透過指令`rake integrate`將檔案copy回`_post`

使用範例如下

`source/_post`有10篇文章，其中有一篇檔名為`2013-03-04-foo-bar.md`是這次要編輯的

透過`rake isolate`來隔離`2013-03-04-foo-bar.md`之外的文章

     rake isolate[foo-bar]

檔名要去掉日期的部份及副檔名, 下完指令後`_post`目錄下只剩`2013-03-04-foo-bar.md`這個檔案


	rake generate

只剩一個檔案，generate速度當然飛快

文章編輯完成，準備發佈前記得透過`rake integrate`把其他檔案從`_stage`目錄copy回來

    rake integrate

此時，所有的檔案已經都回到`_post`目錄，這時必須再對所有內容`generate`一次，就可以進行發佈`rake deploy`了
