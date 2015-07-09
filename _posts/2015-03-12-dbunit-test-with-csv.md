---
published: true
author: Kent Chiu
layout: post
title: "使用csv當dbunit測試的資料格式"
date: 2015-07-07 22:11
tags: 
- dbunit
- csv
- testing
---



Why csv? 

編輯方便，不論是web 還是 IDE，都有很多很好用的 editor可以整 csv進行editing


注意事項

* 需有一個檔名為 `table-ordering.txt` 的檔案，裡面記錄會用到的csv檔名(不能有副檔名)
    ex:
    
    ```
    my_table_1
    my_table_2
    ```
* db的null值，必須用小寫的null表示，(如果NULL是大寫，常常會出現的是日期格式的錯誤!!)
* csv 第一列必須為header列
* 日期型態的值必需用引號包住，格式為 "yyyy-MM-dd HH:mm:ss" ex: "2014-12-14 00:15:22"
