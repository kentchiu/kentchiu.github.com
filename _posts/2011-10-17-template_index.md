---
author: Kent Chiu
published: true
layout: post
title: "這篇文章說明每個模版的功能"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - smarty
  - ecshop
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



基本上，每個template的layout也不用特意去強記，因為，每一個theme里的每一個template(lib,dwt,
wherever…)，總之就是themes目錄下的所有檔案都不一樣。
所以，應該比較用心去了解的反而是將資料往smarty (templates)
pushing的那些php檔。能夠了解每個php送出的資訊，大概也就知道template的能秀出那麼東西了。

dwt應該是**D**ream**W**eaver
**T**emplate的縮寫，而lbi也是dreamweaver裡的一種格式。它可以透過一些特殊的tags達到server
side的include效果。 在dwt檔裡有許多地方都有以下兩組tags


```
<!-- #BeginLibraryItem "/library/xxx.lbi" --><!-- #EndLibraryItem -->

```


```
<!-- TemplateBeginEditable name="xxx" --><!-- TemplateEndEditable -->  

```

第一組tag的功能是include
lbi檔，如果tag有內容，內容應該會與被included的檔案一模一樣，這是因為**某些**editor(dreamweaver)自動include進來的
，但是如果**直接去編輯tag內容(inner
text)是沒用的，頁面並不會相對改變**，惟有直接修改lbi檔的內容，頁面才會改變。

第二組tag不太清楚實際的作用是什麼^[1)](#fn__1)^可以直接拿掉，但tag內容要保留。

以一個簡單的例子來說明這種效果



```
    <b>Hello World!</b>

```



```
    <!-- #BeginLibraryItem "/library/my.lbi" -->
    <b>Hello World!</b>
    <!-- #EndLibraryItem -->
    <!-- TemplateBeginEditable name="foo" -->
    <b>Kent</b>
    <!-- TemplateEndEditable -->  

```

上面的lib會被included到my.dwt內，所以輸出會像這樣：



```
    Hello World!Kent

```

再來，說明一下另一個效果



```
    <!-- #BeginLibraryItem "/library/my.lbi" -->
    <b>--- Hello World! ---</b>
    <!-- #EndLibraryItem -->
    <!-- TemplateBeginEditable name="foo" -->
    <b>Kent</b>
    <!-- TemplateEndEditable -->  

```

my.dwt裡第2行\<b\>Hello
World!\</b\>這個並不會被render，就算去修改它，也沒有用，將第二行改成
**\<b\>— Hello World! —\</b\>**，結果輸出跟之前一樣



```
    Hello World!Kent

```

但如果是去修改lbi檔的內容



```
    <b> *** Hello World! *** </b>

```

輸出就會改變成跟lbi的內容一樣了



```
    *** Hello World! *** Kent

```

這說明了**修改BeginLibraryItem /
\#EndLibraryItem間的內容是沒用的**，一定要去修改lbi的檔。
換句話說，其實可以對dwt檔做以下的簡化

1.  可以移除\<!– TemplateBeginEditable name=“xxx” –\>\<!–
    TemplateEndEditable –\> 這一對tag，但tag的內文要保留
2.  可以移除\<!– \#BeginLibraryItem ”/library/xxx.lbi” –\>\<!–
    \#EndLibraryItem –\> tag的內文，但tag本身要保留

所以，簡化後的dwt檔會像這樣



```
    <!-- #BeginLibraryItem "/library/my.lbi" -->
    <b>Kent</b>

```

有些官方的template本身就是簡化過的(預設的template沒有簡化)，所以可以拿簡化過的template來測試，會比較乾淨

ECShop裡面模版可以透過web畫面進行調整，調整後的layout是紀錄在db，然後把調整的page(ex:index.dwt)內容給置換掉

dwt 模版
--------

備註：範本檔共22個(格式：.dwt)。

提醒：

1.  更改範本檔裡面庫檔的內容是無效的，頁面刷新時，程式自動重新載入庫檔內容到範本檔裡(以庫檔內容為准)。
2.  範本內所有id值為 ECS\_ 開頭的都必須保留(和ajax相關)。
3.  非庫檔內容不可放置到可編輯區域內，否則設置範本時，非庫檔內容將被覆蓋刪除。

  名稱                    說明
  ----------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------
  brand.dwt               商品品牌頁
  article.dwt             文章內容頁
  article\_cat.dwt        文章列表頁
  catalog.dwt             所有分類頁
  category.dwt            商品列表頁
  compare.dwt             商品比較頁
  flow.dwt                購物車和購物流程頁
  gallery.dwt             商品相冊頁
  goods.dwt               商品詳情頁
  group\_buy\_goods.dwt   團購商品詳情頁
  group\_buy\_list.dwt    團購商品列表頁
  index.dwt               首頁
  message.dwt             資訊提示頁
  pick\_out.dwt           選購中心頁
  receive.dwt             收貨確認資訊頁
  respond.dwt             線上支付結果提示資訊頁
  search.dwt              商品搜尋網頁
  snatch.dwt              奪寶奇兵頁
  tag\_cloud.dwt          標籤雲頁
  user\_clips.dwt         用戶中心頁 （包含：歡迎頁，我的留言，我的標籤，收藏商品，缺貨登記列表，添加缺貨登記。）
  user\_passport.dwt      用戶安全頁（包含：會員登錄，會員註冊，找回密碼。）
  user\_transaction.dwt   用戶中心頁 （包含：個人資料，我的紅包，添加紅包，我的訂單，訂單詳情，合併訂單，訂單狀態，商品清單，費用總計，收貨人資訊，支付方式，其他資訊，會員餘額。）

Library
-------

備註：庫檔共40個 (格式 .lbi)
提醒：檔案名儘量保存默認，否則在後臺管理將無法管理庫檔或不可預見錯誤。

  name                       description
  -------------------------- ----------------------------------------------------------------------
  ad\_position.lbi           廣告位
  bought\_goods.lbi          購買過此商品的人購買過哪些商品
  brand\_goods.lbi           品牌的商品
  brands.lbi                 品牌專區
  cart.lbi                   購物車
  cat\_articles.lbi          文章列表
  cat\_goods.lbi             分類下的商品
  category\_tree.lbi         商品分類樹
  comments.lbi               用戶評論列表 （ajax載入comments\_list.lbi。）
  comments\_list.lbi         使用者評論內容
  consignee.lbi              收貨地址表單
  goods\_article.lbi         相關文章
  goods\_attrlinked.lbi      屬性關聯的商品
  goods\_fittings.lbi        相關配件
  goods\_gallery.lbi         商品相冊
  goods\_list.lbi            商品列表
  goods\_related.lbi         相關商品
  goods\_tags.lbi            商品標記
  group\_buy.lbi             首頁團購商品
  help.lbi                   網店幫助
  history.lbi                商品流覽歷史
  invoice\_query.lbi         發貨單查詢
  member.lbi                 會員登錄 (ajax載入member\_info.lbi。)
  member\_info.lbi           會員登錄表單和登錄成功以後使用者帳戶資訊
  new\_articles.lbi          最新文章
  order\_total.lbi           訂單費用總計
  page\_footer.lbi           頁面腳部
  page\_header.lbi           頁面頂部
  pages.lbi                  列表分頁
  recommend\_best.lbi        精品推薦
  recommend\_hot.lbi         熱賣商品
  recommend\_new.lbi         新品推薦
  recommend\_promotion.lbi   促銷商品
  search\_form.lbi           搜索表單
  snatch.lbi                 奪寶奇兵出價表單 (必須被id=“ECS\_SNATCH”包含實現ajax刷新。)
  snatch\_price.lbi          奪寶奇兵最新出價列表 (必須被id=“ECS\_PRICE\_LIST”包含實現ajax刷新。)
  top10.lbi                  銷售排行
  ur\_here.lbi               當前位置
  user\_menu.lbi             使用者中心功能表
  vote.lbi                   線上調查

### brand.dwt (商品品牌頁)

![template_index_article.dwt.gif][]

### brand.dw (商品品牌頁)

![template_index_brand.dwt.gif][]

### article.dw (文章內容頁)

![template_index_article.dwt.gif][]

### article\_cat.dwt (文章列表頁)

![template_index_article_cat.dwt.gif][]

### catalog.dw (所有分類頁)

![template_index_catalog.dwt.gif][]

### category.dwt (商品列表頁)

![template_index_category.dwt.gif][]

### compare.dw (商品比較頁)

![template_index_compare.dwt.gif][]

### flow.dwt (購物車和購物流程頁)

![template_index_flow.dwt.gif][]

### gallery.dw (商品相冊頁)

![template_index_gallery.dwt.gif][]

### goods.dw (商品詳情頁)

![template_index_goods.dwt.gif][]

### group\_buy\_goods.dwt (團購商品詳情頁)

![template_index_group_buy_goods.dwt.gif][]

### group\_buy\_list.dwt (團購商品列表頁)

![template_index_group_buy_list.dwt.gif][]

### index.dw (首頁)

兩張圖的layout沒有完全一致，參考看看就好

![template_index_index.dwt.gif][]

![template_index_index.png][template_index_index.png]

### message.dw ( 資訊提示頁)

![template_index_message.dwt.gif][]

### pick\_out.dwt (選購中心頁)

![template_index_pick_out.dwt.gif][]

### receive.dw (收貨確認資訊頁)

![template_index_receive.dwt.gif][]

### respond.dw (線上支付結果提示資訊頁)

![template_index_respond.dwt.gif][]

### search.dwt (商品搜尋網頁)

![template_index_search.dwt.gif][]

### snatch.dwt (奪寶奇兵頁)

![template_index_snatch.dwt.gif][]

### tag\_cloud.dwt (標籤雲頁)

![template_index_tag_cloud.dwt.gif][]

### user\_clips.dwt (用戶中心頁)

![template_index_user_clips.dwt.gif][]

### user\_passport.dwt (用戶安全頁)

![template_index_user_passport.dwt.gif][]

### user\_transaction.dwt (用戶中心頁)

![template_index_user_transaction.dwt.gif][]





[template_index_article.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_article.dwt.gif
[template_index_brand.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_brand.dwt.gif
[template_index_article.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_article.dwt.gif
[template_index_article_cat.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_article_cat.dwt.gif
[template_index_catalog.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_catalog.dwt.gif
[template_index_category.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_category.dwt.gif
[template_index_compare.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_compare.dwt.gif
[template_index_flow.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_flow.dwt.gif
[template_index_gallery.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_gallery.dwt.gif
[template_index_goods.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_goods.dwt.gif
[template_index_group_buy_goods.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_group_buy_goods.dwt.gif
[template_index_group_buy_list.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_group_buy_list.dwt.gif
[template_index_index.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_index.dwt.gif
[template_index_index.png]: http://blog.kent-chiu.com/images/2011-10-17/template_index_index.png
[template_index_message.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_message.dwt.gif
[template_index_pick_out.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_pick_out.dwt.gif
[template_index_receive.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_receive.dwt.gif
[template_index_respond.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_respond.dwt.gif
[template_index_search.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_search.dwt.gif
[template_index_snatch.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_snatch.dwt.gif
[template_index_tag_cloud.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_tag_cloud.dwt.gif
[template_index_user_clips.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_user_clips.dwt.gif
[template_index_user_passport.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_user_passport.dwt.gif
[template_index_user_transaction.dwt.gif]: http://blog.kent-chiu.com/images/2011-10-17/template_index_user_transaction.dwt.gif
