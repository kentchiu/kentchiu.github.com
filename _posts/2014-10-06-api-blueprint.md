---
published: true
author: Kent Chiu
layout: post
title: "API Blueprint筆記"
date: 2014-10-06 01:20
comments: true
sharing: true
footer: true
tags: 
- rest
- markdown
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




一直以來，都在為了怎麼幫restful api寫文件而煩惱，對於一般的api，很多都會在語言的層面提供，像是python的docstrings，ruby的 doc comment, 或java的javadoc等，可以從method的註解產生api說明，但是restful的api，有其特殊性，一般不會在語法的層級支援 restful api的文件的寫作。

目前評估過幾個solutions有:

1. 從source code的comment產生文件
   [swagger](https://helloreverb.com/developers/swagger)為其中典型的代表，其他還有許多也是利用特定的annatation來結合source code本身的meta data產生文件。這種方案的好處是，可以透過reflection 程式，得到很多資訊，進而減少需要人工寫作的部份，在跟程式的同步性，一致性也比較好。但缺點就是彈性比較不足。
2. 範本檔
   有些熱心的人會提供word，google doc，純文字等的template來當作api文件寫法的基本結構，至於裡面該寫那些東西，大多大同小異。
3. 特定格式
   像[API Buleprint](http://apiblueprint.org/)，有定義出特定的規則系結構，依照它規格的結構與語法進行寫作後，通常會提供工作把api轉成可讀性比較高的結果(通常最後的結果是html格式)
4. 自製
   這是我採用API Buleprint前的方式，我偏好用純文字進行 api的文件撰寫，通常是採用markdown的格式，然後自已參考一些流行的rest api的文件結構，自已弄出一個喜歡的結構，用markdown寫的好處，是可以一般的 text editor就可以直接查看內容，如果需要比較好的呈現效果，也可以轉換成其他的呈現方式。

會採用API Buleprint主要考量有: 

1. 採markdown格式撰寫
2. API Buleprint只定義規則跟結構，不負責render，render有plugin負責
3. 有plugin機制，其中有plugin提供單一的靜態html就可以查看api
4. 原來自製的markdown，對toc跟hyper link效果不是很好，文件一大時，維護跟查找不便




## Notes

1. 撰寫時，最好開`live preview server`，可以馬上做preview及語法檢查
2. 文件比較大時，可以分成好幾個檔，然後再用cat指令多個指令接成一個檔再產生html
3. 把非collection的resource secion放collection resource section前面，讓post(post是放collection resource section，因為沒有{id})可以reuse resource mode，ex，user GET,PUT, DELETE 放 users get, post前面
4. resource的定義有三種格式
   `# <URI template>` or `# <identifier> [<URI template>]` or `# <HTTP request method> <URI template>` , 第三種方式的nested section會變成 Action scetion
   如果action比較少的情況，可以用第二種格式，就不用再定義Action sesion
5. sumlime text有[plugin](https://sublime.wbond.net/packages/API%20Blueprint)可以把文件parse成API Blueprint的語法樹(AST)
6. 目前的版本(Format 1A revision 7) request跟response的屬性沒辦法像parameter那樣說明每個json的屬性，但之後的版本會有，可以先用 markdown的table語法來寫，或用schema section


# resource
- [API Blueprint 規格書](https://github.com/apiaryio/api-blueprint/blob/master/API%20Blueprint%20Specification.md) - 建議仔細讀過，只看tutorail上是不夠的
- <https://github.com/danielgtaylor/aglio> - 把API Blueprint轉成html的工具，轉出來的html需放在server上才有辦法正常render
- <https://sublime.wbond.net/packages/API%20Blueprint> - sublime text 3 的plugin
- <http://tools.ietf.org/html/rfc6570> - url template語法的RFC