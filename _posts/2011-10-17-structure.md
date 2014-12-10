---
author: Kent Chiu
published: true
layout: post
title: "ECShop Structure"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
tags:
  - ecshop
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




ECShop upload 的目錄


```
           ├ activity.php 活動列表
           ├ affiche.php 廣告處理文件
           ├ affiliate.php 生成商品列表
           ├ article.php 文章內容
           ├ article_cat.php文章分類
           ├ auction.php 拍賣前台文件
           ├ brand.php 品牌列表
           ├ captcha.php 生成驗證碼
           ├ catalog.php 列出所以分類及品牌
           ├ category.php 商品分類
           ├ comment.php 提交用戶評論
           ├ compare.php 商品比較程序
           ├ cycle_image.php  輪播圖片程序
           ├ feed.php RSS Feed 生成程序
           ├ flow.php 購物流程
           ├ gallery.php 商品相冊
           ├ goods.php 商品詳情
           ├ goods_script.php 生成商品列表
           ├ group_buy.php 團購商品前台文件
           ├ index.php 首頁文件
           ├ myship.php 支付配送DEMO
           ├ pick_out.php 選購中心
           ├ receive.php 處理收回確認的頁面
           ├ region.php 地區切換程序
           ├ respond.php 支付響應頁面
           ├ robots.txt
           ├ search.php 搜索程序
           ├ sitemaps.php  google sitemap 文件
           ├ snatch.php 奪寶奇兵前台頁面
           ├ tag_cloud.php 標籤雲
           ├ topic.php 專題前台
           ├ user.php 會員中心
           ├ vote.php 調查程序
           ├ wholesale.php 批發前台文件
           ├ admin文件夾
           │   ├ account_log.php 管理中心帳戶變動記錄
           │   ├ admin_logs.php 記錄管理員操作日誌
           │   ├ ads.php 廣告管理程序
           │   ├ adsense.php 站外JS投放的統計程序
           │   ├ ad_position.php廣告位置管理程序
           │   ├ affiliate.php 程序說明
           │   ├ affiliate_ck.php 程序說明
           │   ├ agency.php 管理中心辦事處管理
           │   ├ area_manage.php 地區列表管理文件
           │   ├ article.php 管理中心文章處理程序文件
           │   ├ articlecat.php 文章分類管理程序
           │   ├ article_auto.php
           │   ├ attention_list.php
           │   ├ attribute.php 屬性規格管理
           │   ├ auction.php 管理中心拍賣活動管理
           │   ├ bonus.php 紅包類型的處理
           │   ├ brand.php管理中心品牌管理
           │   ├ captcha_manage.php
           │   ├ card.php 賀卡管理程序
           │   ├ category.php 商品分類管理程序
           │   ├ check_file_priv.php 系統文件檢測
           │   ├ comment_manage.php 用戶評論管理程序
           │   ├ convert.php 轉換程序
           │   ├ cron.php 計劃任務
           │   ├ database.php 數據庫管理
           │   ├ ebao_commend.php 易寶推薦
           │   ├ edit_languages.php 管理中心語言項編輯(前台語言項)
           │   ├ email_list.php 郵件列表管理
           │   ├ favourable.php 管理中心優惠活動管理
           │   ├ flashplay.php
           │   ├ flow_stats.php 綜合流量統計
           │   ├ friend_link.php 友情鏈接管理
           │   ├ gen_goods_script.php 生成顯示商品的js代碼
           │   ├ get_password.php 找回管理員密碼
           │   ├ goods.php 商品管理程序
           │   ├ goods_auto.php
           │   ├ goods_batch.php 商品批量上傳、修改
           │   ├ goods_booking.php 缺貨處理管理程序
           │   ├ goods_export.php
           │   ├ goods_type.php 商品類型管理程序
           │   ├ group_buy.php 管理中心團購商品管理
           │   ├ guest_stats.php 客戶統計
           │   ├ index.php 控制台首頁
           │   ├ integrate.php 第三方程序會員數據整合插件管理程序
           │   ├ magazine_list.php
           │   ├ mail_template.php 管理中心模版管理程序
           │   ├ message.php 管理中心管理員留言程序
           │   ├ navigator.php
           │   ├ order.php 訂單管理
           │   ├ order_stats.php 訂單統計
           │   ├ pack.php 包裝管理程序
           │   ├ payment.php 支付方式管理程序
           │   ├ picture_batch.php 圖片批量處理程序
           │   ├ privilege.php 管理員信息以及權限管理程序
           │   ├ sale_general.php 銷售概況
           │   ├ sale_list.php 銷售明細列表程序
           │   ├ sale_order.php 商品銷售排行
           │   ├ searchengine_stats.php 搜索引擎關鍵字統計
           │   ├ search_log.php
           │   ├ shipping.php 配送方式管理程序
           │   ├ shipping_area.php 配送區域管理程序
           │   ├ shophelp.php 幫助信息管理程序
           │   ├ shopinfo.php 網店信息管理頁面
           │   ├ shop_config.php  管理中心商店設置
           │   ├ sitemap.php 站點地圖生成程序
           │   ├ sms.php 短信模塊 之 控制器
           │   ├ snatch.php 奪寶奇兵管理程序
           │   ├ sql.php sql管理程序
           │   ├ tag_manage.php 後台標籤管理
           │   ├ template.php 管理中心模版管理程序
           │   ├ topic.php 專題管理
           │   ├ users.php 會員管理程序
           │   ├ users_order.php 會員排行統計程序
           │   ├ user_account.php 會員帳目管理(包括預付款，餘額)
           │   ├ user_msg.php 客戶留言
           │   ├ user_rank.php 會員等級管理程序
           │   ├ view_sendlist.php
           │   ├ virtual_card.php 虛擬卡商品管理程序
           │   ├ visit_sold.php 訪問購買比例
           │   ├ vote.php 調查管理程序
           │   ├ wholesale.php  管理中心批發管理
           │   ├ help 的目錄  後台操作幫助文件
           │   ├ images 的目錄
           │   ├ includes 的目錄
           │   │  ├ cls_exchange.php 後台自動操作數據庫的類文件
           │   │  ├ cls_google_sitemap.php Google sitemap 類
           │   │  ├ cls_phpzip.php ZIP 處理類
           │   │  ├ cls_sql_dump.php 數據庫導出類
           │   │  ├ inc_menu.php 管理中心菜單數組
           │   │  ├ init.php 管理中心公用文件
           │   │  ├ lib_goods.php 管理中心商品相關函數
           │   │  ├ lib_main.php 管理中心公用函數庫
           │   │  └ lib_template.php 管理中心模版相關公用函數庫
           │   ├ styles 的目錄
           │   ├ templates 的目錄
           │   └ js 的目錄
           │       ├ colorselector.js
           │       ├ common.js
           │       ├ listtable.js
           │       ├ md5.js
           │       ├ selectzone.js
           │       ├ tab.js
           │       ├ todolist.js
           │       ├ topbar.js
           │       └ validator.js 表單驗證類
           ├ api 的目錄
           │   ├ checkorder.php 檢查訂單 API
           │   ├ cron.php
           │   └ init.php API 公用初始化文件
           ├ cert 的目錄
           ├ data 的目錄
           │   ├ ffiliate.html
           │   ├ goods_script.html
           │   ├ order_print.html
           │   ├ afficheimg 的目錄
           │   ├ brandlogo 的目錄
           │   ├ captcha 的目錄 驗證碼背景圖片存放位置
           │   ├ cardimg 的目錄
           │   ├ feedbackimg 的目錄
           │   ├ images 的目錄
           │   ├ packimg 的目錄
           │   └ sqldata 的目錄
           ├ images 的目錄
           │   └ upload 的目錄
           │        ├ File 文件上傳存放處
           │        ├ Flash flash上傳存放處
           │        ├ Image 圖片上傳存放處
           │        └ Media 視頻上傳存放處
           ├ includes 的目錄
           │   ├ cls_captcha.php 驗證碼圖片類
           │   ├ cls_ecshop.php 基礎類
           │   ├ cls_error.php 用戶級錯誤處理類
           │   ├ cls_iconv.php 字符集轉換類
           │   ├ cls_image.php 後台對上傳文件的處理類(實現圖片上傳，圖片縮小， 增加水印)
           │   ├ cls_json.php JSON 類
           │   ├ cls_mysql.php MYSQL 公用類庫
           │   ├ cls_rss.php RSS 類
           │   ├ cls_session.php SESSION 公用類庫
           │   ├ cls_sms.php 短信模塊 之 模型（類庫）
           │   ├ cls_smtp.php SMTP 郵件類
           │   ├ cls_sql_executor.php SQL語句執行類。
           │   ├ cls_template.php 模版類
           │   ├ cls_transport.php 服務器之間數據傳輸器
           │   ├ inc_constant.php 常量
           │   ├ init.php 前台公用文件
           │   ├ lib.debug.php
           │   ├ lib_article.php 文章及文章分類相關函數庫
           │   ├ lib_clips.php ECSHOP 用戶相關函數庫
           │   ├ lib_code.php 加密解密類
           │   ├ lib_common.php 公用函數庫
           │   ├ lib_goods.php 商品相關函數庫
           │   ├ lib_insert.php 動態內容函數庫
           │   ├ lib_main.php 前台公用函數庫
           │   ├ lib_order.php 購物流程函數庫
           │   ├ lib_passport.php 用戶帳號相關函數庫
           │   ├ lib_payment.php 支付接口函數庫
           │   ├ lib_time.php 時間函數
           │   ├ lib_transaction.php ECSHOP 用戶交易相關函數庫
           │   ├ codetable 的目錄
           │   │    ├ big5-gb.table
           │   │    ├ big5_utf8.php
           │   │    ├ gb-big5.table
           │   │    ├ gb_utf8.php
           │   │    └ ipdata.dat
           │   ├ fckeditor 的目錄 fckeditor編輯器目錄
           │   └ modules 的目錄
           │         ├ convert 的目錄
           │         │    ├ shopex46.php vshopex4.6轉換程序插件
           │         │    └ shopex47.php shopex4.7轉換程序插件
           │         ├ cron 的目錄
           │         │    ├ auto_manage.php 自動上下架管理
           │         │    └ ipdel.php 定期刪除
           │         ├ integrates 的目錄
           │         │    ├ bmforum.php 會員數據處理類
           │         │    ├ discuz.php
           │         │    ├ discuz55.php
           │         │    ├ dvbbs.php
           │         │    ├ ecshop.php
           │         │    ├ integrate.php
           │         │    ├ ipb.php
           │         │    ├ molyx.php
           │         │    ├ phpbb.php
           │         │    ├ phpwind.php
           │         │    ├ phpwind5.php
           │         │    └ vbb.php
           │         ├ payment 的目錄
           │         │    ├ alipay.php 支付寶插件
           │         │    ├ balance.php 餘額支付插件
           │         │    ├ bank.php 銀行匯款（轉帳）插件
           │         │    ├ cappay.php 首信易支付插件
           │         │    ├ chinabank.php 網銀在線插件
           │         │    ├ cncard.php 雲網支付插件
           │         │    ├ cod.php 貨到付款插件
           │         │    ├ ctopay.php Ctopay 支付插件
           │         │    ├ express.php express支付系統插件
           │         │    ├ ips.php ips支付系統插件
           │         │    ├ kuaiqian.php 快錢插件
           │         │    ├ nps.php NPS支付插件
           │         │    ├ pay800.php 800pay 支付寶插件
           │         │    ├ paypal.php 貝寶插件
           │         │    ├ paypalcn.php 貝寶中國插件
           │         │    ├ post.php 郵局匯款插件
           │         │    ├ tenpay.php 財付通插件
           │         │    ├ udpay.php 網匯通插件
           │         │    ├ xpay.php 易付通插件
           │         │    └ yeepay.php YeePay易寶插件
           │         └ shipping 的目錄
           │               ├ cac.php 上門取貨插件
           │               ├ city_express.php 城際快遞插件
           │               ├ ems.php EMS插件
           │               ├ flat.php 郵政包裹插件
           │               ├ fpd.php 到付運費插件
           │               ├ post_express.php 郵政包裹插件
           │               ├ post_mail.php 郵局平郵插件
           │               ├ presswork.php 掛號印刷品插件
           │               ├ sf_express.php 順豐速運 配送方式插件
           │               ├ sto_express.php 申通快遞 配送方式插件
           │               ├ yto.php 圓通速遞插件
           │               └ zto.php 中通速遞插件
           ├ install 的目錄  安裝文件目錄
           ├ js 的目錄
           │   ├ auto_complete.js
           │   ├ calendar.php
           │   ├ common.js
           │   ├ compare.js
           │   ├ global.js
           │   ├ lefttime.js
           │   ├ myship.js
           │   ├ region.js
           │   ├ shopping_flow.js
           │   ├ transport.js
           │   ├ user.js
           │   ├ utils.js
           │   └ calendar 的目錄
           ├ languages 的目錄  語言風格文件
           │   ├ zh_cn 的目錄
           │   │   ├
           │   │   ├ admin 的目錄
           │   │   ├ convert 的目錄
           │   │   ├ cron 的目錄
           │   │   ├ payment 的目錄
           │   │   └ shipping 的目錄
           │   └ zh_tw 的目錄
           │        ├ admin 的目錄
           │        ├ convert 的目錄
           │        ├ cron 的目錄
           │        ├ payment 的目錄
           │        └ shipping 的目錄
           ├ plugins 的目錄
           ├ templates 的目錄
           │   ├ backup 的目錄
           │   │   └ ibrary 的目錄
           │   ├ caches 的目錄
           │   └ compiled 的目錄
           │        └ admin 的目錄
           ├ themes 的目錄
           │   ├ default 的目錄
           │   │   ├ images 的目錄
           │   │   └ library 的目錄
           │   └ sport 的目錄
           ├ wap 的目錄
           │   ├ article.php
           │   ├ brands.php
           │   ├ buy.php
           │   ├ category.php
           │   ├ comment.php
           │   ├ goods.php
           │   ├ goods_list.php
           │   ├ index.php
           │   ├ user.php
           │   ├ includes 的目錄
           │   │   ├ init.php
           │   │   ├ lib_main.php
           │   └ templates 的目錄
           │        ├ article.wml
           │        ├ article_list.wml
           │        ├ brands.wml
           │        ├ buy.wml
           │        ├ category.wml
           │        ├ comment.wml
           │        ├ goods.wml
           │        ├ goods_img.wml
           │        ├ goods_list.wml
           │        ├ index.wml
           │        ├ login.wml
           │        ├ order_list.wml
           │        └ user.wml
           └ widget 的目錄
                ├ blog_sohu.php
                ├ blog_sohu.xhtml
                └ images 的目錄

```

Major Pages
===========

各主要頁面 所用模組圖例

1.  首頁:index.dwt
2.  文章列表:article\_cat.dwt
3.  文章顯示:article.dwt
4.  商品分類:category.dwt
5.  商品比較:compare.dwt
6.  商品詳情:goods.dwt
7.  搜索結果:search\_result.dwt
8.  奪寶奇兵:snatch.dwt

Library files
=============

1.  articles.lbi - 文章列表
2.  article\_info.lbi - 文章內容
3.  article\_list.lbi - 文章列表
4.  best\_goods.lbi - 精品推薦
5.  bought\_goods.lbi - 購買過此商品的人購買過哪些商品
6.  brand\_goods.lbi - 品牌的商品
7.  cart.lbi - 購物車
8.  cart\_view.lbi - 查看購物車
9.  category\_tree.lbi - 商品分類樹
10. cat\_goods.lbi - 分類下的商品
11. comments.lbi - 用戶評論
12. comment\_form.lbi - 發表評論的表單
13. consignee.lbi - 收貨人信息
14. fittings.lbi - 相關配件
15. footer.lbi - 頁腳
16. gallery.lbi - 商品相冊
17. goods\_detail.lbi - 商品詳情
18. goods\_info.lbi - 商品基本資訊
19. goods\_list.lbi - 商品列表
20. help.lbi - 説明內容
21. history.lbi - 歷史記錄
22. hot\_goods.lbi - 熱賣商品
23. invoice\_query.lbi - 發貨單查詢
24. member.lbi - 會員登錄區
25. member\_info.lbi - 會員資訊
26. nav\_main.lbi - 主導航
27. new\_goods.lbi - 新品上架
28. order\_confirm.lbi - 訂單確認
29. order\_detail.lbi - 訂單詳情
30. order\_view.lbi - 訂單資訊
31. package\_card.lbi - 包裝和賀卡
32. pages.lbi - 列表分頁
33. page\_top.lbi - 頁面頂部
34. payment.lbi - 支付方式
35. promotion.lbi - 促銷商品
36. properties.lbi - 商品屬性
37. register\_login.lbi - 購物流程登錄和註冊
38. related\_goods.lbi - 相關商品
39. search\_advanced.lbi - 高級搜索表單
40. search\_form.lbi - 搜索表單
41. search\_result.lbi - 搜索結果
42. shipping.lbi - 配送方式
43. signin.lbi - 會員登錄表單
44. snatch\_bid.lbi - 奪寶奇兵出價表單
45. snatch\_goods.lbi - 奪寶奇兵活動的商品
46. snatch\_list.lbi - 奪寶奇兵活動列表
47. snatch\_price.lbi - 奪寶奇兵價格列表
48. snatch\_result.lbi - 奪寶奇兵活動結果
49. top10.lbi - 銷售排行
50. ur\_here.lbi - 當前位置
51. user\_address.lbi - 會員中心收貨人列表
52. user\_address\_add.lbi - 會員中心添加收貨人
53. user\_booking.lbi - 會員中心用戶缺貨登記
54. user\_booking\_add.lbi - 會員中心用戶添加缺貨登記
55. user\_collect.lbi - 會員中心用戶我的最愛
56. user\_forgetpassword.lbi - 會員中心找回密碼

