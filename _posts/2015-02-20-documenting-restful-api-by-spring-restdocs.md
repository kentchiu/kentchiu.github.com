---
published: true
author: Kent Chiu
layout: post
title: "用spring restdocs為spring restful專案加上文件"
date: 2015-02-02 09:11
tags: 
- spring
- restful
- document
- hateoas
---



Spring Restdocs是Spring restful 專案(spring-hateoas, spring-data-rest)專用的文件產生器.
Spring Restdocs 的特色如下:


- 專案必須符合[REST 成熟度模型 level 3](http://martinfowler.com/articles/richardsonMaturityModel.html), 換句話說就是要使用**HATEOAS**
- 文件的撰寫格式為 [AsciiDoc](http://asciidoctor.org/docs/what-is-asciidoc/), 輸出格式預設為 HTML
- 變動性高的reqeust,response的內容使用 spring-mvc-test 驅動 test cases 產出asciidoctor格式的片段程式碼,之後可視需求 include 到主文件
 

試用了一陣子後，因為以下的原因不用了

1. asciidoc 語法，過於彈性，讓文件多了很多非制式的語法，嚴重的降低了一致性跟可讀性
2. 目前用的 frontend framework,extjs, 對heatos支援不好
3. 文件寫法沒有像[API Blueprint](https://apiblueprint.org/)一樣的標準

API Blueprint新版的規格也快release了，所以還是換回了用 API Blueprint，但受到**Spring Restdocs**的啟發，寫了一個sprint mvc testcase -> markdown的功能，
有效的解決之前API Blueprint過到的難題。

# Resources
- <https://github.com/spring-projects/spring-restdocs> - spring restdocs 專案網站
