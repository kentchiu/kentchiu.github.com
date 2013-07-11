---
author: Kent Chiu
published: 
layout: post
title: "ECShop Database Schema"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ecshop
---



-   there are about 76 tables (ecshop version2.6.2)
-   no view
-   no store procedure
-   no trigger
-   no table constrains, only PK
-   using auto key in most tables


tables order by alpha character
-------------------------------

1.  [ecs_account_log](#ecs_account_log "ecshop:database_schema")
    使用者帳號情況記錄表，包括資金和積分等
2.  [ecs_ad](#ecs_ad "ecshop:database_schema")
    廣告清單配置表，包括站內站外的圖片，文字，flash，代碼廣告
3.  [ecs_ad_position](#ecs_ad_position "ecshop:database_schema")
    廣告位置配置表
4.  [ecs_admin_action](#ecs_admin_action "ecshop:database_schema")
    管理員許可權列表樹
5.  [ecs_admin_log](#ecs_admin_log "ecshop:database_schema")
    管理員操作日誌表
6.  [ecs_admin_message](#ecs_admin_message "ecshop:database_schema")
    管理員留言記錄表
7.  [ecs_admin_user](#ecs_admin_user "ecshop:database_schema")
    管理員資料許可權清單
8.  [ecs_adsense](#ecs_adsense "ecshop:database_schema")
    廣告點擊率統計表
9.  [ecs_affiliate_log](#ecs_affiliate_log "ecshop:database_schema")
    分成相關的表，還沒研究透 (TBD)
10. [ecs_agency](#ecs_agency "ecshop:database_schema") 辦事處資訊
11. [ecs_area_region](#ecs_area_region "ecshop:database_schema")
    記錄表ecs_shipping_area中的shipping_area_name的地區名包括ecs_region中的城市
12. [ecs_article](#ecs_article "ecshop:database_schema") 文章內容表
13. [ecs_article_cat](#ecs_article_cat "ecshop:database_schema")
    文章分類資訊表
14. [ecs_attribute](#ecs_attribute "ecshop:database_schema")
    商品類型屬性工作表，該表記錄的是每個商品類型的所有屬性的配置情況，具體的商品的屬性不在該表
15. [ecs_auction_log](#ecs_auction_log "ecshop:database_schema")
    拍賣出價記錄資訊表
16. [ecs_auto_manage](#ecs_auto_manage "ecshop:database_schema")
    處理文章，商品自動上下線的計畫任務列表；需要安裝計畫任務外掛程式才有效
17. [ecs_bonus_type](#ecs_bonus_type "ecshop:database_schema")
    紅包類型表
18. [ecs_booking_goods](#ecs_booking_goods "ecshop:database_schema")
    缺貨登記的訂購和處理記錄表
19. [ecs_brand](#ecs_brand "ecshop:database_schema")
    商品品牌資訊記錄表
20. [ecs_card](#ecs_card "ecshop:database_schema") 賀卡的配置的資訊
21. [ecs_cart](#ecs_cart "ecshop:database_schema")
    購物車購物資訊記錄表
22. [ecs_cat_recommend](#ecs_cat_recommend "ecshop:database_schema")
    尚無資訊 (TBD)
23. [ecs_category](#ecs_category "ecshop:database_schema")
    商品分類表，記錄商品分類資訊
24. [ecs_collect_goods](#ecs_collect_goods "ecshop:database_schema")
    會員收藏商品的記錄清單，一條記錄一個收藏商品
25. [ecs_comment](#ecs_comment "ecshop:database_schema")
    使用者對文章和產品的評論清單
26. [ecs_crons](#ecs_crons "ecshop:database_schema")
    計畫任務外掛程式安裝配置資訊
27. [ecs_email_list](#ecs_email_list "ecshop:database_schema")
    增加電子雜誌訂閱表
28. [ecs_email_sendlis](#ecs_email_sendlis "ecshop:database_schema")
    增加發送隊列表
29. [ecs_error_log](#ecs_error_log "ecshop:database_schema")
    該表用來記錄頁面觸發計畫任務時失敗所產生的錯誤，從程式來看，目前主要是記錄某計畫任務所對應的外掛程式檔不存在的錯誤
30. [ecs_exchange_goods](#ecs_exchange_goods "ecshop:database_schema")
    尚沒資訊(TBD)
31. [ecs_favourable_activity](#ecs_favourable_activity "ecshop:database_schema")
    優惠活動的配置資訊，優惠活動包括送禮，減免，打折
32. [ecs_feedback](#ecs_feedback "ecshop:database_schema")
    使用者回饋資訊表，包括留言，投訴，諮詢等
33. [ecs_friend_link](#ecs_friend_link "ecshop:database_schema")
    友情連結配置資訊表
34. [ecs_goods](#ecs_goods "ecshop:database_schema") 商品表
35. [ecs_goods_activity](#ecs_goods_activity "ecshop:database_schema")
    拍賣活動和奪寶奇兵活動配置資訊
36. [ecs_goods_article](#ecs_goods_article "ecshop:database_schema")
    文章關聯產品表，即文章中提到的相關產品
37. [ecs_goods_attr](#ecs_goods_attr "ecshop:database_schema")
    具體商品的屬性工作表
38. [ecs_goods_cat](#ecs_goods_cat "ecshop:database_schema")
    商品的擴展分類
39. [ecs_goods_gallery](#ecs_goods_gallery "ecshop:database_schema")
    商品相冊表，只出現在頁面的商品相冊中
40. [ecs_goods_type](#ecs_goods_type "ecshop:database_schema")
    商品類型表，該表每條記錄就是一個商品類型
41. [ecs_group_goods](#ecs_group_goods "ecshop:database_schema")
    該表應該是商品配件配置表
42. [ecs_keywords](#ecs_keywords "ecshop:database_schema")
    頁面搜索關鍵字搜索記錄
43. [ecs_link_goods](#ecs_link_goods "ecshop:database_schema")
    關聯商品資訊表，關聯商品是什麼意思還沒研究明白
44. [ecs_mail_template](#ecs_mail_template "ecshop:database_schema")
    各種郵件的範本配置範本包括雜誌範本
45. [ecs_member_price](#ecs_member_price "ecshop:database_schema")
    商品不按照會員的折扣定價，而是再單獨為不同的會員等級定的價
46. [ecs_nav](#ecs_nav "ecshop:database_schema")
    上中下3個巡覽列的顯示配置
47. [ecs_order_action](#ecs_order_action "ecshop:database_schema")
    對訂單操作日誌表
48. [ecs_order_goods](#ecs_order_goods "ecshop:database_schema")
    訂單的商品資訊，注：訂單的商品資訊基本都是從購物車所對應的表中取來的。
49. [ecs_order_info](#ecs_order_info "ecshop:database_schema")
    訂單的配送，賀卡等詳細資訊
50. [ecs_pack](#ecs_pack "ecshop:database_schema") 商品包裝資訊配置表
51. [ecs_package_goods](#ecs_package_goods "ecshop:database_schema")
    尚無資訊(TBD)
52. [ecs_pay_log](#ecs_pay_log "ecshop:database_schema")
    系統支付記錄
53. [ecs_payment](#ecs_payment "ecshop:database_schema")
    安裝的支付方式配置資訊
54. [ecs_plugins](#ecs_plugins "ecshop:database_schema") 外掛(插件)
55. [ecs_region](#ecs_region "ecshop:database_schema") 地區清單
56. [ecs_searchengine](#ecs_searchengine "ecshop:database_schema")
    搜尋引擎訪問記錄
57. [ecs_sessions](#ecs_sessions "ecshop:database_schema")
    session記錄表
58. [ecs_sessions_data](#ecs_sessions_data "ecshop:database_schema")
    session資料表（超過255位元組的session內容會保存在該表）
59. [ecs_shipping](#ecs_shipping "ecshop:database_schema")
    配送方式配置資訊表
60. [ecs_shipping_area](#ecs_shipping_area "ecshop:database_schema")
    配送方式所屬的配送區域和配送費用資訊
61. [ecs_shop_config](#ecs_shop_config "ecshop:database_schema")
    全站配置資訊表
62. [ecs_snatch_log](#ecs_snatch_log "ecshop:database_schema")
    奪寶奇兵出價記錄表
63. [ecs_stats](#ecs_stats "ecshop:database_schema") 訪問資訊記錄表
64. [ecs_tag](#ecs_tag "ecshop:database_schema") 商品的標記
65. [ecs_template](#ecs_template "ecshop:database_schema")
    範本設置資料表
66. [ecs_topic](#ecs_topic "ecshop:database_schema") 專題活動配置表
67. [ecs_user_account](#ecs_user_account "ecshop:database_schema")
    用戶資金流動表，包括提現和充值
68. [ecs_user_address](#ecs_user_address "ecshop:database_schema")
    收貨人的信息表
69. [ecs_user_bonus](#ecs_user_bonus "ecshop:database_schema")
    已經發送的紅包資訊清單
70. [ecs_user_feed](#ecs_user_feed "ecshop:database_schema")
    尚無資訊 (TBD)
71. [ecs_user_rank](#ecs_user_rank "ecshop:database_schema")
    會員等級配置資訊
72. [ecs_users](#ecs_users "ecshop:database_schema")
    使用者帳號資訊，包括資金和積分等
73. [ecs_virtual_card](#ecs_virtual_card "ecshop:database_schema")
    虛擬卡卡號庫
74. [ecs_vote](#ecs_vote "ecshop:database_schema") 網站調查資訊記錄表
75. [ecs_volume_price](#ecs_volume_price "ecshop:database_schema")
    尚無資訊 (TBD)
76. [ecs_vote_log](#ecs_vote_log "ecshop:database_schema")
    投票記錄表
77. [ecs_vote_option](#ecs_vote_option "ecshop:database_schema")
    投票的選項內容表
78. [ecs_wholesale](#ecs_wholesale "ecshop:database_schema")
    批發方案表

ER diagram
----------

![ecshop_erd_all2.jpg][]

![ecshop_erd_all.jpg][]

tables group by functionality
-----------------------------

Because the list is base on functionality, therefore, there are some
tables would show in difference group more then one time.

### user/account

1.  [ecs_order_info](#ecs_order_info "ecshop:database_schema")
    訂單的配送，賀卡等詳細資訊
2.  [ecs_user_bonus](#ecs_user_bonus "ecshop:database_schema")
    已經發送的紅包資訊清單
3.  [ecs_user_address](#ecs_user_address "ecshop:database_schema")
    收貨人的信息表
4.  [ecs_user_account](#ecs_user_account "ecshop:database_schema")
    用戶資金流動表，包括提現和充值
5.  [ecs_users](#ecs_users "ecshop:database_schema")
    使用者帳號資訊，包括資金和積分等
6.  [ecs_account_log](#ecs_account_log "ecshop:database_schema")
    使用者帳號情況記錄表，包括資金和積分等
7.  [ecs_user_feed](#ecs_user_feed "ecshop:database_schema")
    尚無資訊 (TBD)
8.  [ecs_user_rank](#ecs_user_rank "ecshop:database_schema")
    會員等級配置資訊
9.  [ecs_group_goods](#ecs_group_goods "ecshop:database_schema")
    該表應該是商品配件配置表

![ecshop_erd_user.jpg][]

### admin

1.  [ecs_admin_action](#ecs_admin_action "ecshop:database_schema")
    管理員許可權列表樹
2.  [ecs_agency](#ecs_agency "ecshop:database_schema") 辦事處資訊
3.  [ecs_admin_user](#ecs_admin_user "ecshop:database_schema")
    管理員資料許可權清單
4.  [ecs_admin_message](#ecs_admin_message "ecshop:database_schema")
    管理員留言記錄表
5.  [ecs_admin_log](#ecs_admin_log "ecshop:database_schema")
    管理員操作日誌表

![ecshop_erd_admin.jpg][]

### article

1.  [ecs_article](#ecs_article "ecshop:database_schema") 文章內容表
2.  [ecs_article_cat](#ecs_article_cat "ecshop:database_schema")
    文章分類資訊表
3.  [ecs_comment](#ecs_comment "ecshop:database_schema")
    使用者對文章和產品的評論清單
4.  [ecs_auto_manage](#ecs_auto_manage "ecshop:database_schema")
    處理文章，商品自動上下線的計畫任務列表；需要安裝計畫任務外掛程式才有效
5.  [ecs_goods_article](#ecs_goods_article "ecshop:database_schema")
    文章關聯產品表，即文章中提到的相關產品

![ecshop_erd_article.jpg][]

### adsense

1.  [ecs_ad](#ecs_ad "ecshop:database_schema")
    廣告清單配置表，包括站內站外的圖片，文字，flash，代碼廣告
2.  [ecs_ad_position](#ecs_ad_position "ecshop:database_schema")
    廣告位置配置表
3.  [ecs_adsense](#ecs_adsense "ecshop:database_schema")
    廣告點擊率統計表

![ecshop_erd_adsence.jpg][]

### affiliate

1.  [ecs_affiliate_log](#ecs_affiliate_log "ecshop:database_schema")
    分成相關的表，還沒研究透
2.  [ecs_order_info](#ecs_order_info "ecshop:database_schema")
    訂單的配送，賀卡等詳細資訊
3.  [ecs_users](#ecs_users "ecshop:database_schema")
    使用者帳號資訊，包括資金和積分等

![ecshop_erd_affiliate.jpg][]

### auction

1.  [ecs_auction_log](#ecs_auction_log "ecshop:database_schema")
    拍賣出價記錄資訊表
2.  [ecs_users](#ecs_users "ecshop:database_schema")
    使用者帳號資訊，包括資金和積分等
3.  [ecs_goods](#ecs_goods "ecshop:database_schema") 商品表
4.  [ecs_goods_activity](#ecs_goods_activity "ecshop:database_schema")
    拍賣活動和奪寶奇兵活動配置資訊
5.  [ecs_snatch_log](#ecs_snatch_log "ecshop:database_schema")
    奪寶奇兵出價記錄表

![ecshop_erd_auction.jpg][]

### shapping car

1.  [ecs_cart](#ecs_cart "ecshop:database_schema")
    購物車購物資訊記錄表
2.  [ecs_favourable_activity](#ecs_favourable_activity "ecshop:database_schema")
    優惠活動的配置資訊，優惠活動包括送禮，減免，打折
3.  [ecs_attribute](#ecs_attribute "ecshop:database_schema")
    商品類型屬性工作表，該表記錄的是每個商品類型的所有屬性的配置情況，具體的商品的屬性不在該表
4.  [ecs_goods_attr](#ecs_goods_attr "ecshop:database_schema")
    具體商品的屬性工作表
5.  [ecs_goods_type](#ecs_goods_type "ecshop:database_schema")
    商品類型表，該表每條記錄就是一個商品類型

![ecshop_erd_cart.jpg][]

### goods category

1.  [ecs_category](#ecs_category "ecshop:database_schema")
    商品分類表，記錄商品分類資訊
2.  [ecs_goods_attr](#ecs_goods_attr "ecshop:database_schema")
    具體商品的屬性工作表
3.  [ecs_goods](#ecs_goods "ecshop:database_schema") 商品表
4.  [ecs_goods_cat](#ecs_goods_cat "ecshop:database_schema")
    商品的擴展分類

![ecshop_erd_category.jpg][]

### goods

1.  [ecs_brand](#ecs_brand "ecshop:database_schema")
    商品品牌資訊記錄表
2.  [ecs_goods_article](#ecs_goods_article "ecshop:database_schema")
    文章關聯產品表，即文章中提到的相關產品
3.  [ecs_wholesale](#ecs_wholesale "ecshop:database_schema")
    批發方案表
4.  [ecs_volume_price](#ecs_volume_price "ecshop:database_schema")
    尚無資訊 (TBD)
5.  [ecs_goods](#ecs_goods "ecshop:database_schema") 商品表
6.  [ecs_collect_goods](#ecs_collect_goods "ecshop:database_schema")
    會員收藏商品的記錄清單，一條記錄一個收藏商品
7.  [ecs_booking_goods](#ecs_booking_goods "ecshop:database_schema")
    缺貨登記的訂購和處理記錄表
8.  [ecs_cart](#ecs_cart "ecshop:database_schema")
    購物車購物資訊記錄表
9.  [ecs_member_price](#ecs_member_price "ecshop:database_schema")
    商品不按照會員的折扣定價，而是再單獨為不同的會員等級定的價
10. [ecs_goods_type](#ecs_goods_type "ecshop:database_schema")
    商品類型表，該表每條記錄就是一個商品類型
11. [ecs_order_goods](#ecs_order_goods "ecshop:database_schema")
    訂單的商品資訊，注：訂單的商品資訊基本都是從購物車所對應的表中取來的。
12. [ecs_package_goods](#ecs_package_goods "ecshop:database_schema")
    尚無資訊(TBD)
13. [ecs_link_goods](#ecs_link_goods "ecshop:database_schema")
    關聯商品資訊表，關聯商品是什麼意思還沒研究明白
14. [ecs_comment](#ecs_comment "ecshop:database_schema")
    使用者對文章和產品的評論清單
15. [ecs_goods_gallery](#ecs_goods_gallery "ecshop:database_schema")
    商品相冊表，只出現在頁面的商品相冊中
16. [ecs_virtual_card](#ecs_virtual_card "ecshop:database_schema")
    虛擬卡卡號庫
17. [ecs_group_goods](#ecs_group_goods "ecshop:database_schema")
    該表應該是商品配件配置表

![ecshop_erd_goods.jpg][]

### mail

1.  [ecs_email_list](#ecs_email_list "ecshop:database_schema")
    增加電子雜誌訂閱表
2.  [ecs_email_sendlis](#ecs_email_sendlis "ecshop:database_schema")
    增加發送隊列表
3.  [ecs_mail_template](#ecs_mail_template "ecshop:database_schema")
    各種郵件的範本配置範本包括雜誌範本

![ecshop_erd_mail.jpg][]

### order

1.  [ecs_pay_log](#ecs_pay_log "ecshop:database_schema")
    系統支付記錄
2.  [ecs_shipping](#ecs_shipping "ecshop:database_schema")
    配送方式配置資訊表
3.  [ecs_payment](#ecs_payment "ecshop:database_schema")
    安裝的支付方式配置資訊
4.  [ecs_card](#ecs_card "ecshop:database_schema") 賀卡的配置的資訊
5.  [ecs_user_bonus](#ecs_user_bonus "ecshop:database_schema")
    已經發送的紅包資訊清單
6.  [ecs_users](#ecs_users "ecshop:database_schema")
    使用者帳號資訊，包括資金和積分等
7.  [ecs_order_info](#ecs_order_info "ecshop:database_schema")
    訂單的配送，賀卡等詳細資訊
8.  [ecs_goods](#ecs_goods "ecshop:database_schema") 商品表
9.  [ecs_favourable_activity](#ecs_favourable_activity "ecshop:database_schema")
    優惠活動的配置資訊，優惠活動包括送禮，減免，打折
10. [ecs_pack](#ecs_pack "ecshop:database_schema") 商品包裝資訊配置表
11. [ecs_affiliate_log](#ecs_affiliate_log "ecshop:database_schema")
    分成相關的表，還沒研究透
12. [ecs_order_action](#ecs_order_action "ecshop:database_schema")
    對訂單操作日誌表
13. [ecs_order_goods](#ecs_order_goods "ecshop:database_schema")
    訂單的商品資訊，注：訂單的商品資訊基本都是從購物車所對應的表中取來的。

![ecshop_erd_order.jpg][]

### region

1.  [ecs_agency](#ecs_agency "ecshop:database_schema") 辦事處資訊
2.  [ecs_region](#ecs_region "ecshop:database_schema") 地區清單
3.  [ecs_area_region](#ecs_area_region "ecshop:database_schema")
    記錄表ecs_shipping_area中的shipping_area_name的地區名包括ecs_region中的城市
4.  [ecs_shipping](#ecs_shipping "ecshop:database_schema")
    配送方式配置資訊表
5.  [ecs_shipping_area](#ecs_shipping_area "ecshop:database_schema")
    配送方式所屬的配送區域和配送費用資訊

![ecshop_erd_region.jpg][]

### vote

1.  [ecs_vote_log](#ecs_vote_log "ecshop:database_schema")
    投票記錄表
2.  [ecs_vote_option](#ecs_vote_option "ecshop:database_schema")
    投票的選項內容表
3.  [ecs_vote](#ecs_vote "ecshop:database_schema") 網站調查資訊記錄表

![ecshop_erd_vote.jpg][]

### uncategorized

1.  [ecs_crons](#ecs_crons "ecshop:database_schema")
    計畫任務外掛程式安裝配置資訊
2.  [ecs_error_log](#ecs_error_log "ecshop:database_schema")
    該表用來記錄頁面觸發計畫任務時失敗所產生的錯誤，從程式來看，目前主要是記錄某計畫任務所對應的外掛程式檔不存在的錯誤
3.  [ecs_friend_link](#ecs_friend_link "ecshop:database_schema")
    友情連結配置資訊表
4.  [ecs_keywords](#ecs_keywords "ecshop:database_schema")
    頁面搜索關鍵字搜索記錄
5.  [ecs_nav](#ecs_nav "ecshop:database_schema")
    上中下3個巡覽列的顯示配置
6.  [ecs_plugins](#ecs_plugins "ecshop:database_schema") 外掛(插件)
7.  [ecs_searchengine](#ecs_searchengine "ecshop:database_schema")
    搜尋引擎訪問記錄
8.  [ecs_sessions](#ecs_sessions "ecshop:database_schema")
    session記錄表
9.  [ecs_sessions_data](#ecs_sessions_data "ecshop:database_schema")
    session資料表（超過255位元組的session內容會保存在該表）
10. [ecs_shop_config](#ecs_shop_config "ecshop:database_schema")
    全站配置資訊表
11. [ecs_bonus_type](#ecs_bonus_type "ecshop:database_schema")
    紅包類型表
12. [ecs_cat_recommend](#ecs_cat_recommend "ecshop:database_schema")
    尚無資訊 (TBD)
13. [ecs_stats](#ecs_stats "ecshop:database_schema") 訪問資訊記錄表
14. [ecs_tag](#ecs_tag "ecshop:database_schema") 商品的標記
15. [ecs_template](#ecs_template "ecshop:database_schema")
    範本設置資料表
16. [ecs_topic](#ecs_topic "ecshop:database_schema") 專題活動配置表
17. [ecs_feedback](#ecs_feedback "ecshop:database_schema")
    使用者回饋資訊表，包括留言，投訴，諮詢等

table schema
------------

轉載說明:這份schema的中文說明部份主要是來自已ecshop官方fourm裡一份熱心網友的貢獻(因為找不到出處了，所以沒給原post的link)，我只是將它轉成繁體中文。

### <a id='ecs_account_log'>ecs_account_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_account_log` (
        `log_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `user_id` mediumint(8) unsigned NOT NULL COMMENT '用戶登錄後保存在session中的id號，跟users表中的user_id對應',
        `user_money` decimal(10,2) NOT NULL COMMENT '使用者該筆記錄的餘額',
        `frozen_money` decimal(10,2) NOT NULL COMMENT '被凍結的資金',
        `rank_points` mediumint(9) NOT NULL COMMENT '等級積分，跟消費積分是分開的',
        `pay_points` mediumint(9) NOT NULL COMMENT '消費積分，跟等級積分是分開的',
        `change_time` int(10) unsigned NOT NULL COMMENT '該筆操作發生的時間',
        `change_desc` varchar(255) NOT NULL COMMENT '該筆操作的備註，一般是，充值或者提現。也可是是管理員後臺寫的任何在備註',
        `change_type` tinyint(3) unsigned NOT NULL COMMENT '操作類型，0為充值，1為提現，2為管理員調節，99為其他類型',
        PRIMARY KEY (`log_id`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='使用者帳號情況記錄表，包括資金和積分等' AUTO_INCREMENT=42 ;
```

### <a id='ecs_ad'>ecs_ad</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_ad` (
        `ad_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `position_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '0,站外廣告；從1開始代表的是該廣告所處的廣告位，同表ad_position中的欄位position_id的值',
        `media_type` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '廣告類型，0，圖片；1，flash;2,代碼；3，文字',
        `ad_name` varchar(60) NOT NULL COMMENT '該條廣告記錄的廣告名稱',
        `ad_link` varchar(255) NOT NULL COMMENT '廣告連結位址',
        `ad_code` text NOT NULL COMMENT '廣告連結的表現，文字廣告就是文字或圖片和flash就是它們的地址，代碼廣告就是代碼內容',
        `start_time` int(11) NOT NULL DEFAULT '0' COMMENT '廣告開始時間',
        `end_time` int(11) NOT NULL DEFAULT '0' COMMENT '廣告結束時間',
        `link_man` varchar(60) NOT NULL COMMENT '廣告連絡人',
        `link_email` varchar(60) NOT NULL COMMENT '廣告連絡人的郵箱',
        `link_phone` varchar(60) NOT NULL COMMENT '廣告連絡人的電話',
        `click_count` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該廣告點擊數',
        `enabled` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '該廣告是否關閉，1，開啟；0，關閉；關閉後廣告將不再有效，直至重新開啟',
        PRIMARY KEY (`ad_id`),
        KEY `position_id` (`position_id`),
        KEY `enabled` (`enabled`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='廣告清單配置表，包括站內站外的圖片，文字，flash，代碼廣告' AUTO_INCREMENT=6 ;
```

### <a id='ecs_admin_action'>ecs_admin_action</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_admin_action` (
        `action_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `parent_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '該id項的父id，對應本表的action_id欄位',
        `action_code` varchar(20) NOT NULL COMMENT '代表權限的英文字串，對應漢文在語言檔中，如果該欄位有某個字串，就表示有該許可權',
        PRIMARY KEY (`action_id`),
        KEY `parent_id` (`parent_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='管理員許可權列表樹' AUTO_INCREMENT=104 ;
```

### <a id='ecs_admin_log'>ecs_admin_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_admin_log` (
        `log_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `log_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '寫日誌時間',
        `user_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '該日誌所記錄的操作者id，同ecs_admin_user的user_id',
        `log_info` varchar(255) NOT NULL COMMENT '管理操作內容',
        `ip_address` varchar(15) NOT NULL COMMENT '管理者登錄ip',
        PRIMARY KEY (`log_id`),
        KEY `log_time` (`log_time`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='管理員操作日誌表' AUTO_INCREMENT=158 ;
```

### <a id='ecs_admin_message'>ecs_admin_message</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_admin_message` (
        `message_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `sender_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '發送該留言的管理員id，同ecs_admin_user的user_id',
        `receiver_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '接收消息的管理員id，同ecs_admin_user的user_id，如果是給多個管理員發送，則同一個消息給每個管理員id發送一條',
        `sent_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '留言發送時間',
        `read_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '留言閱讀時間',
        `readed` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '留言是否閱讀，1，已閱讀；0，未閱讀',
        `deleted` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '留言是否已經是否已經被刪除，1，已刪除；0，未刪除',
        `title` varchar(150) NOT NULL COMMENT '留言的主題',
        `message` text NOT NULL COMMENT '留言的內容',
        PRIMARY KEY (`message_id`),
        KEY `sender_id` (`sender_id`,`receiver_id`),
        KEY `receiver_id` (`receiver_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='管理員留言記錄表' AUTO_INCREMENT=7 ;
```

### <a id='ecs_admin_user'>ecs_admin_user</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_admin_user` (
        `user_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號，管理員代號',
        `user_name` varchar(60) NOT NULL COMMENT '管理員登錄名',
        `email` varchar(60) NOT NULL COMMENT '管理員郵箱',
        `password` varchar(32) NOT NULL COMMENT '管理員登錄秘密加密串',
        `add_time` int(11) NOT NULL DEFAULT '0' COMMENT '管理員添加時間',
        `last_login` int(11) NOT NULL DEFAULT '0' COMMENT '管理員最後一次登錄時間',
        `last_ip` varchar(15) NOT NULL COMMENT '管理員最後一次登錄ip',
        `action_list` text NOT NULL COMMENT '管理員管理許可權列表',
        `nav_list` text NOT NULL COMMENT '管理員巡覽列配置項',
        `lang_type` varchar(50) NOT NULL,
        `agency_id` smallint(5) unsigned NOT NULL COMMENT '該管理員負責的辦事處的id，同ecs_agency的agency_id欄位。如果管理員沒負責辦事處，則此處為0',
        `todolist` longtext COMMENT '記事本記錄的資料',
        PRIMARY KEY (`user_id`),
        KEY `user_name` (`user_name`),
        KEY `agency_id` (`agency_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='管理員資料許可權清單' AUTO_INCREMENT=4 ;
```

### <a id='ecs_adsense'>ecs_adsense</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_adsense` (
        `from_ad` smallint(5) NOT NULL DEFAULT '0' COMMENT '廣告代號，-1是站外廣告，如果是站內廣告則為ecs_ad的ad_id',
        `referer` varchar(255) NOT NULL COMMENT '頁面來源',
        `clicks` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '點擊率',
        KEY `from_ad` (`from_ad`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='廣告點擊率統計表';
```

ecs_adsense.from_ad :
廣告代號，-1是站外廣告，**如果是站內廣告則為ecs_ad的ad_id**

這個table沒有primary key

### <a id='ecs_ad_position'>ecs_ad_position</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_ad_position` (
        `position_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '廣告位自增id',
        `position_name` varchar(60) NOT NULL COMMENT '廣告位名稱',
        `ad_width` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '廣告位寬度',
        `ad_height` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '廣告位高度',
        `position_desc` varchar(255) NOT NULL COMMENT '廣告位描述',
        `position_style` text NOT NULL COMMENT '廣告位元範本代碼',
        PRIMARY KEY (`position_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='廣告位置配置表' AUTO_INCREMENT=2 ;
```

### <a id='ecs_affiliate_log'>ecs_affiliate_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_affiliate_log` (
        `log_id` mediumint(8) NOT NULL AUTO_INCREMENT,
        `order_id` mediumint(8) NOT NULL,
        `time` int(10) NOT NULL,
        `user_id` mediumint(8) NOT NULL, COMMENT='需確定是admin_user還是一般的users',
        `user_name` varchar(60) DEFAULT NULL,
        `money` decimal(10,2) NOT NULL DEFAULT '0.00',
        `point` int(10) NOT NULL DEFAULT '0',
        `separate_type` tinyint(1) NOT NULL DEFAULT '0',
        PRIMARY KEY (`log_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='分成相關的表，還沒研究透' AUTO_INCREMENT=1 ;
```

### <a id='ecs_agency'>ecs_agency</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_agency` (
        `agency_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '辦事處ID',
        `agency_name` varchar(255) NOT NULL COMMENT '辦事處名字',
        `agency_desc` text NOT NULL COMMENT '辦事處描述',
        PRIMARY KEY (`agency_id`),
        KEY `agency_name` (`agency_name`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='辦事處資訊' AUTO_INCREMENT=5 ;
```

### <a id='ecs_area_region'>ecs_area_region</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_area_region` (
        `shipping_area_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '配送區域的id號，等同於ecs_shipping_area的shipping_area_id的值',
        `region_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '地區清單，等同於ecs_region的region_id',
        PRIMARY KEY (`shipping_area_id`,`region_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='記錄表ecs_shipping_area中的shipping_area_name的地區名包括ecs_region中的城市';
```

### <a id='ecs_article'>ecs_article</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_article` (
        `article_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cat_id` smallint(5) NOT NULL DEFAULT '0' COMMENT '該文章的分類，同ecs_article_cat的cat_id,如果不在，將自動成為保留類型而不能刪除',
        `title` varchar(150) NOT NULL COMMENT '文章題目',
        `content` longtext NOT NULL COMMENT '文章內容',
        `author` varchar(30) NOT NULL COMMENT '文章作者',
        `author_email` varchar(60) NOT NULL COMMENT '文章作者的email',
        `keywords` varchar(255) NOT NULL COMMENT '文章的關鍵字',
        `article_type` tinyint(1) unsigned NOT NULL DEFAULT '2' COMMENT '文章類型，0，普通；1，置頂；2和大於2的，為保留文章，保留文章不能刪除',
        `is_open` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否顯示。1，顯示；0，不顯示',
        `add_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '文章添加時間',
        `file_url` varchar(255) NOT NULL COMMENT '上傳檔或者外部檔的url',
        `open_type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '0,正常；當該欄位為1或者2時，會在文章最後添加一個連結“相關下載”，連接位址等於file_url的值；但程式在此處有bug',
        `link` varchar(255) NOT NULL COMMENT '該文章標題所引用的連接，如果該項有值將不能顯示文章內容，即該表中content的值',
        PRIMARY KEY (`article_id`),
        KEY `cat_id` (`cat_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='文章內容表' AUTO_INCREMENT=11 ;
```

### <a id='ecs_article_cat'>ecs_article_cat</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_article_cat` (
        `cat_id` smallint(5) NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cat_name` varchar(255) NOT NULL COMMENT '分類名稱',
        `cat_type` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '分類類型；1，普通分類；2，系統分類；3，網店信息；4，幫助分類；5，網店幫助',
        `keywords` varchar(255) NOT NULL COMMENT '分類關鍵字',
        `cat_desc` varchar(255) NOT NULL COMMENT '分類說明文字',
        `sort_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '分類顯示順序',
        `show_in_nav` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否在巡覽列顯示；0，否；1，是',
        `parent_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '父節點id，取值於該表cat_id欄位',
        PRIMARY KEY (`cat_id`),
        KEY `cat_type` (`cat_type`),
        KEY `sort_order` (`sort_order`),
        KEY `parent_id` (`parent_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='文章分類資訊表' AUTO_INCREMENT=7 ;
```

### <a id='ecs_attribute'>ecs_attribute</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_attribute` (
        `attr_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cat_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品類型，同ecs_goods_type的cat_id',
        `attr_name` varchar(60) NOT NULL COMMENT '屬性名稱',
        `attr_input_type` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '當添加商品時，該屬性的添加類別；0，為手工輸入；1，為選擇輸入；2，為多行文本輸入',
        `attr_type` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '屬性是否多選；0，否；1，是；如果可以多選，則可以自訂屬性，並且可以根據值的不同定不同的價',
        `attr_values` text NOT NULL COMMENT '如果attr_input_type為1，即選擇輸入，則attr_name對應的值的取值就是該欄位的值',
        `attr_index` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '屬性是否可以檢索；0，不需要檢索；1，關鍵字檢索；2，範圍檢索；該屬性應該是如果檢索的話，可以通過該屬性找到有該屬性的商品',
        `sort_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '屬性顯示的順序，數位越大越靠前，如果數位一樣則按id順序',
        `is_linked` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否關聯；0，不關聯；1，關聯；如果關聯，那麼用戶在購買該商品時，具有有該屬性相同值的商品將被推薦給用戶',
        `attr_group` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '屬性分組，相同的為一個屬性組。該值應該取自ecs_goods_type的attr_group的值的順序',
        PRIMARY KEY (`attr_id`),
        KEY `cat_id` (`cat_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品類型屬性工作表，該表記錄的是每個商品類型的所有屬性的配置情況，具體的商品的屬性不在該表' AUTO_INCREMENT=175 ;
```

### <a id='ecs_auction_log'>ecs_auction_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_auction_log` (
        `log_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `act_id` mediumint(8) unsigned NOT NULL COMMENT '拍賣活動的id，取值於ecs_goods_activity的act_id欄位',
        `bid_user` mediumint(8) unsigned NOT NULL COMMENT '出價的用戶id，取值於ecs_users的user_id',
        `bid_price` decimal(10,2) unsigned NOT NULL COMMENT '出價價格',
        `bid_time` int(10) unsigned NOT NULL COMMENT '出價時間',
        PRIMARY KEY (`log_id`),
        KEY `act_id` (`act_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='拍賣出價記錄資訊表' AUTO_INCREMENT=3 ;
```

### <a id='ecs_auto_manage'>ecs_auto_manage</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_auto_manage` (
        `item_id` mediumint(8) NOT NULL COMMENT '如果是商品就是ecs_goods的goods_id，如果是文章就是ecs_article的article_id',
        `type` varchar(10) NOT NULL COMMENT 'goods是商品，article是文章',
        `starttime` int(10) NOT NULL COMMENT '上線時間',
        `endtime` int(10) NOT NULL COMMENT '下線時間',
        PRIMARY KEY (`item_id`,`type`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='處理文章，商品自動上下線的計畫任務列表；需要安裝計畫任務外掛程式才有效';
```

ecs_auto_manage.item_id :
如果是商品就是ecs_goods的goods_id，如果是文章就是ecs_article的article_id,
由ecs_auto_manage.item_id = goods or article決定

### <a id='ecs_bonus_type'>ecs_bonus_type</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_bonus_type` (
        `type_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '紅包類型流水號',
        `type_name` varchar(60) NOT NULL COMMENT '紅包名稱',
        `type_money` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '紅包所值的金額',
        `send_type` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '紅包發送類型.0,按使用者如會員等級,會員名稱發放;1,按商品類別發送;2,按訂單金額所達到的額度發送;3,線下發送',
        `min_amount` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '如果是按金額發送紅包,該項是最小金額.即只要購買超過該金額的商品都可以領到紅包',
        `max_amount` decimal(10,2) unsigned NOT NULL DEFAULT '0.00',
        `send_start_date` int(11) NOT NULL DEFAULT '0' COMMENT '紅包發送的開始時間',
        `send_end_date` int(11) NOT NULL DEFAULT '0' COMMENT '紅包發送的結束時間',
        `use_start_date` int(11) NOT NULL DEFAULT '0' COMMENT '紅包可以使用的開始時間',
        `use_end_date` int(11) NOT NULL DEFAULT '0' COMMENT '紅包可以使用的結束時間',
        `min_goods_amount` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '可以使用該紅包的商品的最低價格.即只要達到該價格的商品才可以使用紅包',
        PRIMARY KEY (`type_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='紅包類型表' AUTO_INCREMENT=6 ;
```

### <a id='ecs_booking_goods'>ecs_booking_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_booking_goods` (
        `rec_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '登記該缺貨記錄的使用者的id，取值ecs_users的user_id',
        `email` varchar(60) NOT NULL COMMENT '頁面填的使用者的email，默認取值於ecs_users的email',
        `link_man` varchar(60) NOT NULL COMMENT '頁面填的使用者的姓名，默認取值於ecs_users的consignee ',
        `tel` varchar(60) NOT NULL COMMENT '頁面填的使用者的電話，預設取值於ecs_users的tel',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '缺貨登記的商品id，取值於ecs_goods的 goods_id',
        `goods_desc` varchar(255) NOT NULL COMMENT '缺貨登記時留的訂購描述',
        `goods_number` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '訂購數量',
        `booking_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '缺貨登記的時間',
        `is_dispose` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否已經被處理',
        `dispose_user` varchar(30) NOT NULL COMMENT '處理該缺貨登記的管理員用戶名，取值於session,該session取值於ecs_admin_user的user_name',
        `dispose_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '處理的時間',
        `dispose_note` varchar(255) NOT NULL COMMENT '處理時管理員留的備註',
        PRIMARY KEY (`rec_id`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='缺貨登記的訂購和處理記錄表' AUTO_INCREMENT=4 ;
```

### <a id='ecs_brand'>ecs_brand</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_brand` (
        `brand_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `brand_name` varchar(60) NOT NULL COMMENT '品牌名稱',
        `brand_logo` varchar(80) NOT NULL COMMENT '上傳的該品牌公司logo圖片',
        `brand_desc` text NOT NULL COMMENT '品牌描述',
        `site_url` varchar(255) NOT NULL COMMENT '品牌的網址',
        `sort_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '品牌在前臺頁面的顯示順序，數位越大越靠後',
        `is_show` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '該品牌是否顯示，0，否；1，顯示',
        PRIMARY KEY (`brand_id`),
        KEY `is_show` (`is_show`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品品牌資訊記錄表' AUTO_INCREMENT=9 ; 
```

### <a id='ecs_card'>ecs_card</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_card` (
        `card_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `card_name` varchar(120) NOT NULL COMMENT '賀卡名稱',
        `card_img` varchar(255) NOT NULL COMMENT '賀卡圖紙的名稱',
        `card_fee` decimal(6,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '賀卡所需費用',
        `free_money` decimal(6,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '訂單達到該欄位的值後使用此賀卡免費',
        `card_desc` varchar(255) NOT NULL COMMENT '賀卡的描述',
        PRIMARY KEY (`card_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='賀卡的配置的資訊' AUTO_INCREMENT=2 ;
```

### <a id='ecs_cart'>ecs_cart</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_cart` (
        `rec_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '用戶登錄id，取自session，',
        `session_id` char(32) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL COMMENT '登錄的sessionid，如果該用戶退出，該sessionid對應的購物車中的所有記錄都將被刪除',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品的id，取自表goods的goods_id',
        `goods_sn` varchar(60) NOT NULL COMMENT '商品的貨號，取自表goods的goods_sn',
        `goods_name` varchar(120) NOT NULL COMMENT '商品的名稱，取自表goods的goods_name',
        `market_price` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '商品的市場價，取自表goods的market_price',
        `goods_price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '商品的本店價，取自表goods的shop_price',
        `goods_number` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品的購買數量，在購物車時，實際庫存不減少',
        `goods_attr` text NOT NULL COMMENT '商品的屬性，中括弧裡是該屬性特有的價格',
        `is_real` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '取自ecs_goods的is_real',
        `extension_code` varchar(30) NOT NULL COMMENT '商品的擴展屬性，取自ecs_goods的extension_code',
        `parent_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該商品的父商品id，沒有該值為0，有的話那該商品就是該id的配件',
        `rec_type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '購物車商品類型，0，普通；1，團夠；2，拍賣；3，奪寶奇兵',
        `is_gift` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '是否是贈品，0，否；其他，是參加優惠活動的id，取值於ecs_favourable_activity 的act_id',
        `can_handsel` tinyint(3) unsigned NOT NULL DEFAULT '0',
        `goods_attr_id` mediumint(8) NOT NULL COMMENT '該商品的屬性的id，取自goods_attr的goods_attr_id，如果有多個，只記錄了最後一個，可能是個bug',
        PRIMARY KEY (`rec_id`),
        KEY `session_id` (`session_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='購物車購物資訊記錄表' AUTO_INCREMENT=82 ;
```

1.  ecs_cart很多欄位的值都是從ecs_goods取得
2.  session_id :
    登錄的sessionid，如果該用戶退出，該sessionid對應的購物車中的所有記錄都將被刪除
    → 這個需要弄清楚操作流程

ecs_cart.is_gift 由名字上來看，應該是boolen值，但實際上是這樣:
是否是贈品，0，否；其他，是參加優惠活動的id，取值於ecs_favourable_activity
的act_id

### <a id='ecs_cat_recommend'>ecs_cat_recommend</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_cat_recommend` (
          `cat_id` smallint(5) NOT NULL,
          `recommend_type` tinyint(1) NOT NULL,
          PRIMARY KEY  (`cat_id`,`recommend_type`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

### <a id='ecs_category'>ecs_category</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_category` (
        `cat_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cat_name` varchar(90) NOT NULL COMMENT '分類名稱',
        `keywords` varchar(255) NOT NULL COMMENT '分類的關鍵字，可能是為了搜索',
        `cat_desc` varchar(255) NOT NULL COMMENT '分類描述',
        `parent_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '該分類的父id，取值於該表的cat_id欄位',
        `sort_order` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '該分類在頁面顯示的順序，數位越大順序越靠後；同數位，id在前的先顯示',
        `template_file` varchar(50) NOT NULL COMMENT '不確定欄位，按名字和表設計猜，應該是該分類的單獨範本檔的名字',
        `measure_unit` varchar(15) NOT NULL COMMENT '該分類的計量單位',
        `show_in_nav` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否顯示在巡覽列，0，不；1，顯示在巡覽列',
        `style` varchar(150) NOT NULL COMMENT '該分類的單獨的樣式表的包括檔案名部分的檔路徑',
        `is_show` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否在前臺頁面顯示，1，顯示；0，不顯示',
        `grade` tinyint(4) NOT NULL DEFAULT '0' COMMENT '該分類的最高和最低價之間的價格分級，當大於1時，會根據最大最小價格區間分成區間，會在頁面顯示價格範圍，如0-300,300-600,600-900這種',
        `filter_attr` smallint(6) NOT NULL DEFAULT '0' COMMENT '如果該欄位有值，則該分類將還會按照該值對應在表goods_attr的goods_attr_id所對應的屬性篩選，如，封面顏色下有紅，黑分類篩選 ',
        PRIMARY KEY (`cat_id`),
        KEY `parent_id` (`parent_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品分類表，記錄商品分類資訊' AUTO_INCREMENT=9 ;
```

### <a id='ecs_collect_goods'>ecs_collect_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_collect_goods` (
        `rec_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '收藏記錄的自增id',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該條收藏記錄的會員id，取值於ecs_users的user_id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '收藏的商品id，取值於ecs_goods的goods_id',
        `add_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '收藏時間',
        `is_attention` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否關注該收藏商品，1，是；0，否',
        PRIMARY KEY (`rec_id`),
        KEY `user_id` (`user_id`),
        KEY `goods_id` (`goods_id`),
        KEY `is_attention` (`is_attention`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='會員收藏商品的記錄清單，一條記錄一個收藏商品' AUTO_INCREMENT=3 ;
```

### <a id='ecs_comment'>ecs_comment</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_comment` (
        `comment_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '用戶評論的自增id',
        `comment_type` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '用戶評論的類型；0，評論的是商品；1，評論的是文章',
        `id_value` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '文章或者商品的id，文章對應的是ecs_article 的article_id；商品對應的是ecs_goods的goods_id',
        `email` varchar(60) NOT NULL COMMENT '評論時提交的email地址，默認取的ecs_users的email',
        `user_name` varchar(60) NOT NULL COMMENT '評論該文章或商品的人的名稱，取值ecs_users的user_name',
        `content` text NOT NULL COMMENT '評論的內容',
        `comment_rank` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '該文章或者商品的星級；只有1到5星；由數位代替；其中5是代表5星',
        `add_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '評論的時間',
        `ip_address` varchar(15) NOT NULL COMMENT '評論時的用戶ip',
        `status` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '是否被管理員批准顯示，1，是；0，未批准顯示',
        `parent_id` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '評論的父節點；取值該表的comment_id欄位；如果該欄位為0，則是一個普通評論，否則該條評論就是該欄位的值所對應的評論的回復',
        `user_id` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '發表該評論的用戶的用戶id，取值於ecs_users的user_id',
        PRIMARY KEY (`comment_id`),
        KEY `parent_id` (`parent_id`),
        KEY `id_value` (`id_value`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='使用者對文章和產品的評論清單' AUTO_INCREMENT=5 ;
```

### <a id='ecs_crons'>ecs_crons</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_crons` (
        `cron_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cron_code` varchar(20) NOT NULL COMMENT '該外掛程式檔在相應路徑下的不包括''.php''部分的檔案名，運行該外掛程式將通過該欄位的值尋找將運行的檔',
        `cron_name` varchar(120) NOT NULL COMMENT '計畫任務的名稱',
        `cron_desc` text COMMENT '計畫人物的描述',
        `cron_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '應該是用了設置計畫任務執行的順序的，即當同時觸發2個任務時先執行哪一個，如果一樣應該是id在前的先執行暫不確定',
        `cron_config` text NOT NULL COMMENT '對每次處理的資料的數量的值，類型，名稱序列化；比如刪幾天的日誌，每次執行幾個商品或文章的處理',
        `thistime` int(10) NOT NULL DEFAULT '0' COMMENT '該計畫任務上次被執行的時間',
        `nextime` int(10) NOT NULL COMMENT '該計畫任務下次被執行的時間',
        `day` tinyint(2) NOT NULL COMMENT '如果該欄位有值，則計畫任務將在每月的這一天執行該計畫人物',
        `week` varchar(1) NOT NULL COMMENT '如果該欄位有值，則計畫任務將在每週的這一天執行該計畫人物',
        `hour` varchar(2) NOT NULL COMMENT '如果該欄位有值，則該計畫任務將在每天的這個小時段執行該計畫任務',
        `minute` varchar(255) NOT NULL COMMENT '如果該欄位有值，則該計畫任務將在每小時的這個分鐘段執行該計畫任務，該欄位的值可以多個，用空格間隔',
        `enable` tinyint(1) NOT NULL DEFAULT '1' COMMENT '該計畫任務是否開啟；0，關閉；1，開啟',
        `run_once` tinyint(1) NOT NULL DEFAULT '0' COMMENT '執行後是否關閉，這個關閉的意思還得再研究下',
        `allow_ip` varchar(100) NOT NULL COMMENT '允許運行該計畫人物的伺服器ip',
        `alow_files` varchar(255) NOT NULL COMMENT '運行觸發該計畫人物的檔列表可多個值，為空代表所有許可的檔都可以',
        PRIMARY KEY (`cron_id`),
        KEY `nextime` (`nextime`),
        KEY `enable` (`enable`),
        KEY `cron_code` (`cron_code`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='計畫任務外掛程式安裝配置資訊' AUTO_INCREMENT=4 ;
```

### <a id='ecs_email_list'>ecs_email_list</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_email_list` (
        `id` mediumint(8) NOT NULL AUTO_INCREMENT COMMENT '郵件訂閱的自增id',
        `email` varchar(60) NOT NULL COMMENT '郵件訂閱所填的郵箱地址',
        `stat` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否確認，可以用戶確認也可以管理員確認；0，未確認；1，已確認',
        `hash` varchar(10) NOT NULL COMMENT '郵箱確認的驗證碼，系統生成後發送到使用者郵箱，用戶驗證啟動時通過該值判斷是否合法；主要用來防止非法驗證郵箱',
        PRIMARY KEY (`id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='增加電子雜誌訂閱表' AUTO_INCREMENT=5 ;
```

### <a id='ecs_email_sendlist'>ecs_email_sendlist</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_email_sendlist` (
        `id` mediumint(8) NOT NULL AUTO_INCREMENT COMMENT '郵件發送佇列自增id',
        `email` varchar(100) NOT NULL COMMENT '該郵件將要發送到的郵箱地址',
        `template_id` mediumint(8) NOT NULL COMMENT '該郵件的範本id，取值於ecs_mail_templates的template_id',
        `email_content` text NOT NULL COMMENT '郵件發送的內容',
        `error` tinyint(1) NOT NULL DEFAULT '0' COMMENT '錯誤次數，不知幹什麼用的，猜應該是發送郵件的失敗記錄',
        `pri` tinyint(10) NOT NULL COMMENT '該郵件發送的優先順序；0，普通；1，高',
        `last_send` int(10) NOT NULL COMMENT '上一次發送的時間',
        PRIMARY KEY (`id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='增加發送隊列表' AUTO_INCREMENT=18 ;
```

### <a id='ecs_error_log'>ecs_error_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_error_log` (
        `id` int(10) NOT NULL AUTO_INCREMENT COMMENT '計畫任務錯誤自增id',
        `info` varchar(255) NOT NULL COMMENT '錯誤詳細資訊',
        `file` varchar(100) NOT NULL COMMENT '產生錯誤的執行檔的絕對路徑',
        `time` int(10) NOT NULL COMMENT '錯誤發生的時間',
        PRIMARY KEY (`id`),
        KEY `time` (`time`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='該表用來記錄頁面觸發計畫任務時失敗所產生的錯誤，從程式來看，目前主要是記錄某計畫任務所對應的外掛程式檔不存在的錯誤' AUTO_INCREMENT=1 ;
```

### <a id='ecs_exchange_goods'>ecs_exchange_goods</a>


```
    CREATE TABLE IF NOT EXISTS `ecs_exchange_goods` (
      `goods_id` mediumint(8) unsigned NOT NULL default '0',
      `exchange_integral` int(10) unsigned NOT NULL default '0',
      `is_exchange` tinyint(1) unsigned NOT NULL default '0',
      `is_hot` tinyint(1) unsigned NOT NULL default '0',
      PRIMARY KEY  (`goods_id`)
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

好像是2.6x才有的table，尚無資訊

### <a id='ecs_favourable_activity'>ecs_favourable_activity</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_favourable_activity` (
        `act_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '優惠活動的自增id',
        `act_name` varchar(255) NOT NULL COMMENT '優惠活動的活動名稱',
        `start_time` int(10) unsigned NOT NULL COMMENT '活動的開始時間',
        `end_time` int(10) unsigned NOT NULL COMMENT '活動的結束時間',
        `user_rank` varchar(255) NOT NULL COMMENT '可以參加活動的使用者資訊，取值於ecs_user_rank的rank_id；其中0是非會員，其他是相應的會員等級；多個值用逗號分隔',
        `act_range` tinyint(3) unsigned NOT NULL COMMENT '優惠範圍；0，全部商品；1，按分類；2，按品牌；3，按商品',
        `act_range_ext` varchar(255) NOT NULL COMMENT '根據優惠活動範圍的不同，該處意義不同；但是都是優惠範圍的約束；如，如果是商品，該處是商品的id，如果是品牌，該處是品牌的id',
        `min_amount` decimal(10,2) unsigned NOT NULL COMMENT '訂單達到金額下限，才參加活動',
        `max_amount` decimal(10,2) unsigned NOT NULL COMMENT '參加活動的訂單金額下限，0，表示沒有上限',
        `act_type` tinyint(3) unsigned NOT NULL COMMENT '參加活動的優惠方式；0，送贈品或優惠購買；1，現金減免；價格打折優惠',
        `act_type_ext` decimal(10,2) unsigned NOT NULL COMMENT '如果是送贈品，該處是允許的最大數量，0，無數量限制；現今減免，則是減免金額，單位元；打折，是折扣值，100算，8折就是80',
        `gift` text NOT NULL COMMENT '如果有特惠商品，這裡是序列化後的特惠商品的id,name,price資訊;取值於ecs_goods的goods_id，goods_name，價格是添加活動時填寫的',
        `sort_order` tinyint(3) unsigned NOT NULL COMMENT '活動在優惠活動頁面顯示的先後順序，數位越大越靠後',
        PRIMARY KEY (`act_id`),
        KEY `act_name` (`act_name`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='優惠活動的配置資訊，優惠活動包括送禮，減免，打折' AUTO_INCREMENT=5 ;
```

### <a id='ecs_feedback'>ecs_feedback</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_feedback` (
        `msg_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '回饋資訊自增id',
        `parent_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '父節點，取自該表msg_id；回饋該值為0；回復回饋為節點id',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '回饋的用戶的id',
        `user_name` varchar(60) NOT NULL COMMENT '回饋的用戶的用戶名',
        `user_email` varchar(60) NOT NULL COMMENT '回饋的用戶的郵箱',
        `msg_title` varchar(200) NOT NULL COMMENT '回饋的標題，回復為reply',
        `msg_type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '回饋的類型，0，留言；1，投訴；2，詢問；3，售後；4，求購',
        `msg_content` text NOT NULL COMMENT '回饋的內容',
        `msg_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '回饋的時間',
        `message_img` varchar(255) NOT NULL DEFAULT '0' COMMENT '用戶上傳的檔的地址',
        `order_id` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '該回饋關聯的訂單id，由使用者提交，取值於 ecs_order_info的order_id；0，為無匹配；',
        PRIMARY KEY (`msg_id`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='使用者回饋資訊表，包括留言，投訴，諮詢等' AUTO_INCREMENT=7 ;
```

### <a id='ecs_friend_link'>ecs_friend_link</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_friend_link` (
        `link_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '友情連結自增id',
        `link_name` varchar(255) NOT NULL COMMENT '友情連結的名稱，img的alt的內容;',
        `link_url` varchar(255) NOT NULL COMMENT '友情連結網站的連結位址',
        `link_logo` varchar(255) NOT NULL COMMENT '友情連結的logo',
        `show_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '在頁面的顯示順序',
        PRIMARY KEY (`link_id`),
        KEY `show_order` (`show_order`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='友情連結配置資訊表' AUTO_INCREMENT=3 ;
```

### <a id='ecs_goods'>ecs_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods` (
        `goods_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '商品的自增id',
        `cat_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品所屬商品分類id，取值ecs_category的cat_id',
        `goods_sn` varchar(60) NOT NULL COMMENT '商品的唯一貨號',
        `goods_name` varchar(120) NOT NULL COMMENT '商品的名稱',
        `goods_name_style` varchar(60) NOT NULL DEFAULT '+' COMMENT '商品名稱顯示的樣式；包括顏色和字體樣式；格式如#ff00ff+strong',
        `click_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '商品點擊數',
        `brand_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '品牌id，取值於ecs_brand 的brand_id',
        `provider_name` varchar(100) NOT NULL COMMENT '供貨人的名稱，程式還沒實現該功能',
        `goods_number` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品庫存數量',
        `goods_weight` decimal(10,3) unsigned NOT NULL DEFAULT '0.000' COMMENT '商品的重量，以千克為單位',
        `market_price` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '市場售價',
        `shop_price` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '本店售價',
        `promote_price` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '促銷價格',
        `promote_start_date` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '促銷價格開始日期',
        `promote_end_date` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '促銷價結束日期',
        `warn_number` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '商品報警數量',
        `keywords` varchar(255) NOT NULL COMMENT '商品關鍵字，放在商品頁的關鍵字中，為搜尋引擎收錄用',
        `goods_brief` varchar(255) NOT NULL COMMENT '商品的簡短描述',
        `goods_desc` text NOT NULL COMMENT '商品的詳細描述',
        `goods_thumb` varchar(255) NOT NULL COMMENT '商品在前臺顯示的微縮圖片，如在分類篩選時顯示的小圖片',
        `goods_img` varchar(255) NOT NULL COMMENT '商品的實際大小圖片，如進入該商品頁時介紹商品屬性所顯示的大圖片',
        `original_img` varchar(255) NOT NULL COMMENT '應該是上傳的商品的原始圖片',
        `is_real` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '是否是實物，1，是；0，否；比如虛擬卡就為0，不是實物',
        `extension_code` varchar(30) NOT NULL COMMENT '商品的擴展屬性，比如像虛擬卡',
        `is_on_sale` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '該商品是否開放銷售，1，是；0，否',
        `is_alone_sale` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否能單獨銷售，1，是；0，否；如果不能單獨銷售，則只能作為某商品的配件或者贈品銷售',
        `integral` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '購買該商品可以使用的積分數量，估計應該是用積分代替金額消費；但程式好像還沒有實現該功能',
        `add_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '商品的添加時間',
        `sort_order` smallint(4) unsigned NOT NULL DEFAULT '0' COMMENT '應該是商品的顯示順序，不過該版程式中沒實現該功能',
        `is_delete` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '商品是否已經刪除，0，否；1，已刪除',
        `is_best` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否是精品；0，否；1，是',
        `is_new` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否是新品；0，否；1，是',
        `is_hot` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否熱銷，0，否；1，是',
        `is_promote` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否特價促銷；0，否；1，是',
        `bonus_type_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '購買該商品所能領到的紅包類型',
        `last_update` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最近一次更新商品配置的時間',
        `goods_type` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品所屬類型id，取值表goods_type的cat_id',
        `seller_note` varchar(255) NOT NULL COMMENT '商品的商家備註，僅商家可見',
        `give_integral` int(11) NOT NULL DEFAULT '-1' COMMENT '購買該商品時每筆成功交易贈送的積分數量。',
        PRIMARY KEY (`goods_id`),
        KEY `goods_sn` (`goods_sn`),
        KEY `cat_id` (`cat_id`),
        KEY `last_update` (`last_update`),
        KEY `brand_id` (`brand_id`),
        KEY `goods_weight` (`goods_weight`),
        KEY `promote_end_date` (`promote_end_date`),
        KEY `promote_start_date` (`promote_start_date`),
        KEY `goods_number` (`goods_number`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品表' AUTO_INCREMENT=35 ;
```

### <a id='ecs_goods_activity'>ecs_goods_activity</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_activity` (
        `act_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `act_name` varchar(255) NOT NULL COMMENT '促銷活動的名稱',
        `act_desc` text NOT NULL COMMENT '促銷活動的描述',
        `act_type` tinyint(3) unsigned NOT NULL,
        `goods_id` mediumint(8) unsigned NOT NULL COMMENT '參加活動的id，取值於ecs_goods的goods_id',
        `goods_name` varchar(255) NOT NULL COMMENT '商品的名稱，取值於ecs_goods的goods_name',
        `start_time` int(10) unsigned NOT NULL COMMENT '活動開始時間',
        `end_time` int(10) unsigned NOT NULL COMMENT '活動結束時間',
        `is_finished` tinyint(3) unsigned NOT NULL COMMENT '活動是否結束，0，結束；1，未結束',
        `ext_info` text NOT NULL COMMENT '序列化後的促銷活動的配置資訊，包括最低價，最高價，出價幅度，保證金等',
        PRIMARY KEY (`act_id`),
        KEY `act_name` (`act_name`,`act_type`,`goods_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='拍賣活動和奪寶奇兵活動配置資訊表' AUTO_INCREMENT=5 ;
```

### <a id='ecs_goods_article'>ecs_goods_article</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_article` (
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品id，取自ecs_goods的goods_id',
        `article_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '文章id，取自 ecs_article 的article_id',
        `admin_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '猜想是管理員的id，但是程式中似乎沒有提及到',
        PRIMARY KEY (`goods_id`,`article_id`,`admin_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='文章關聯產品表，即文章中提到的相關產品';
```

### <a id='ecs_goods_attr'>ecs_goods_attr</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_attr` (
        `goods_attr_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該具體屬性屬於的商品，取值於ecs_goods的goods_id',
        `attr_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '該具體屬性屬於的屬性類型的id，取自ecs_attribute 的attr_id',
        `attr_value` text NOT NULL COMMENT '該具體屬性的值',
        `attr_price` varchar(255) NOT NULL COMMENT '該屬性對應在商品原價格上要加的價格',
        PRIMARY KEY (`goods_attr_id`),
        KEY `goods_id` (`goods_id`),
        KEY `attr_id` (`attr_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='具體商品的屬性工作表' AUTO_INCREMENT=62 ;
```

### <a id='ecs_goods_cat'>ecs_goods_cat</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_cat` (
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品id, 取值自ecs_goods.goods_id',
        `cat_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '商品分類id, 取值自ecs_category.cat_id',
        PRIMARY KEY (`goods_id`,`cat_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='商品的擴展分類';
```

### <a id='ecs_goods_gallery'>ecs_goods_gallery</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_gallery` (
        `img_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '商品相冊自增id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '圖片屬於商品的id',
        `img_url` varchar(255) NOT NULL COMMENT '實際圖片url',
        `img_desc` varchar(255) NOT NULL COMMENT '圖片說明資訊',
        `thumb_url` varchar(255) NOT NULL COMMENT '微縮圖片url',
        `img_original` varchar(255) NOT NULL COMMENT '根據名字猜，應該是上傳的圖片檔的最原始的檔的url',
        PRIMARY KEY (`img_id`),
        KEY `goods_id` (`goods_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品相冊表，只出現在頁面的商品相冊中' AUTO_INCREMENT=23 ;
```

### <a id='ecs_goods_type'>ecs_goods_type</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_goods_type` (
        `cat_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `cat_name` varchar(60) NOT NULL COMMENT '商品類型名',
        `enabled` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '類型狀態，1，為可用；0為不可用；不可用的類型，在添加商品的時候選擇商品屬性將不可選',
        `attr_group` varchar(255) NOT NULL COMMENT '商品屬性分組，將一個商品類型的屬性分成組，在顯示的時候也是按組顯示。該欄位的值顯示在屬性的前一行，像標題的作用',
        PRIMARY KEY (`cat_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品類型表，該表每條記錄就是一個商品類型' AUTO_INCREMENT=10 ;
```

### <a id='ecs_group_goods'>ecs_group_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_group_goods` (
        `parent_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '父商品id, 從ecs_goods表取goods_id值',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '配件商品id, 從ecs_goods表取goods_id值',
        `goods_price` decimal(10,2) unsigned NOT NULL DEFAULT '0.00' COMMENT '配件商品的價格',
        `admin_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '添加該配件的管理員的id',
        PRIMARY KEY (`parent_id`,`goods_id`,`admin_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='該表應該是商品配件配置表';
```

### <a id='ecs_keywords'>ecs_keywords</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_keywords` (
        `date` date NOT NULL DEFAULT '0000-00-00' COMMENT '搜索日期',
        `searchengine` varchar(20) NOT NULL COMMENT '搜尋引擎，默認是ecshop',
        `keyword` varchar(90) NOT NULL COMMENT '搜索關鍵字，即使用者填寫的搜索內容',
        `count` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '搜索次數，按天累加',
        PRIMARY KEY (`date`,`searchengine`,`keyword`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='頁面搜索關鍵字搜索記錄';
```

### <a id='ecs_link_goods'>ecs_link_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_link_goods` (
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品id,從ecs_goods取good_id值',
        `link_goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '被關聯的商品的id,從ecs_goods取good_id值',
        `is_double` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否是雙向關聯；0，否；1，是',
        `admin_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '添加此關聯商品資訊的管理員id,從ecs_admin_user取user_id',
        PRIMARY KEY (`goods_id`,`link_goods_id`,`admin_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='關聯商品資訊表，關聯商品是什麼意思還沒研究明白';
```

### <a id='ecs_mail_templates'>ecs_mail_templates</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_mail_templates` (
        `template_id` tinyint(1) unsigned NOT NULL AUTO_INCREMENT COMMENT '郵件範本自增id',
        `template_code` varchar(30) NOT NULL COMMENT '範本字串名稱，主要用於外掛程式言語包時匹配語言包檔等用途',
        `is_html` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '郵件是否是html格式；0，否；1，是',
        `template_subject` varchar(200) NOT NULL COMMENT '該郵件範本的郵件主題',
        `template_content` text NOT NULL COMMENT '郵件範本的內容',
        `last_modify` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最後一次修改範本的時間',
        `last_send` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最近一次發送的時間，好像僅在雜誌才記錄',
        `type` varchar(10) NOT NULL COMMENT '該郵件範本的郵件類型；共2個類型；magazine，雜誌訂閱；template，關注訂閱',
        PRIMARY KEY (`template_id`),
        UNIQUE KEY `template_code` (`template_code`),
        KEY `type` (`type`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='各種郵件的範本配置範本包括雜誌範本' AUTO_INCREMENT=13 ;
```

### <a id='ecs_member_price'>ecs_member_price</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_member_price` (
        `price_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '折扣價自增id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品的id,從ecs_goods_info取goods_id值',
        `user_rank` tinyint(3) NOT NULL DEFAULT '0' COMMENT '會員登記id，取值ecs_user_rank',
        `user_price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '指定商品對指定會員等級的固定定價價格，單位元',
        PRIMARY KEY (`price_id`),
        KEY `goods_id` (`goods_id`,`user_rank`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='商品不按照會員的折扣定價，而是再單獨為不同的會員等級定的價；' AUTO_INCREMENT=3 ;
```

### <a id='ecs_nav'>ecs_nav</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_nav` (
        `id` mediumint(8) NOT NULL AUTO_INCREMENT COMMENT '導航配置自增id',
        `ctype` varchar(10) DEFAULT NULL,
        `cid` smallint(5) unsigned DEFAULT NULL,
        `name` varchar(255) NOT NULL COMMENT '導航顯示標題',
        `ifshow` tinyint(1) NOT NULL COMMENT '是否顯示',
        `vieworder` tinyint(1) NOT NULL COMMENT '頁面顯示順序，數位越大越靠後',
        `opennew` tinyint(1) NOT NULL COMMENT '導航連結頁面是否在新視窗打開，1，是；其他，否',
        `url` varchar(255) NOT NULL COMMENT '連結的頁面位址',
        `type` varchar(10) NOT NULL COMMENT '處於巡覽列的位置，top為頂部；middle為中間；bottom,為底部',
        PRIMARY KEY (`id`),
        KEY `type` (`type`),
        KEY `ifshow` (`ifshow`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='上中下3個巡覽列的顯示配置' AUTO_INCREMENT=17 ;
```

### <a id='ecs_order_action'>ecs_order_action</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_order_action` (
        `action_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '流水號',
        `order_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '被操作的交易號,取值自ecs_order_info.order_id',
        `action_user` varchar(30) NOT NULL COMMENT '好像並非取值ecs_users，但不清楚是從那取值的',
        `order_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '作何操作.0，未確認；1，已確認；2，已取消；3，無效；4，退貨；',
        `shipping_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '發貨狀態。0，未發貨；1，已發貨；2，已收貨；3，備貨中',
        `pay_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '支付狀態.0,未付款;1,付款中;2,已付款;',
        `action_note` varchar(255) NOT NULL COMMENT '操作備註',
        `log_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '操作時間',
        PRIMARY KEY (`action_id`),
        KEY `order_id` (`order_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='對訂單操作日誌表' AUTO_INCREMENT=18 ;
```

ecs_order_action.action_user好像並非ecs_users

### <a id='ecs_order_goods'>ecs_order_goods</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_order_goods` (
        `rec_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '訂單商品資訊自增id',
        `order_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '訂單商品資訊對應的詳細資訊id，取值order_info的order_id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品的的id，取值表ecs_goods 的goods_id',
        `goods_name` varchar(120) NOT NULL COMMENT '商品的名稱，取值表ecs_goods ',
        `goods_sn` varchar(60) NOT NULL COMMENT '商品的唯一貨號，取值ecs_goods ',
        `goods_number` smallint(5) unsigned NOT NULL DEFAULT '1' COMMENT '商品的購買數量',
        `market_price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '商品的市場售價，取值ecs_goods ',
        `goods_price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '商品的本店售價，取值ecs_goods ',
        `goods_attr` text NOT NULL COMMENT '購買該商品時所選擇的屬性；',
        `send_number` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '當不是實物時，是否已發貨，0，否；1，是',
        `is_real` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否是實物，0，否；1，是；取值ecs_goods ',
        `extension_code` varchar(30) NOT NULL COMMENT '商品的擴展屬性，比如像虛擬卡。取值ecs_goods ',
        `parent_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '父商品id，取值於ecs_cart的parent_id；如果有該值則是值多代表的物品的配件',
        `is_gift` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '是否是贈品，0，否；其他，是參加優惠活動的id，取值於ecs_favourable_activity 的act_id(跟xecs_cart 的is_gift跟其一樣)',
        PRIMARY KEY (`rec_id`),
        KEY `order_id` (`order_id`),
        KEY `goods_id` (`goods_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='訂單的商品資訊，注：訂單的商品資訊基本都是從購物車所對應的表中取來的。' AUTO_INCREMENT=27 ;
```

### <a id='ecs_order_info'>ecs_order_info</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_order_info` (
        `order_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '訂單詳細資訊自增id',
        `order_sn` varchar(20) NOT NULL COMMENT '訂單號，唯一',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '用戶id，同ecs_users的user_id',
        `order_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '訂單狀態。0，未確認；1，已確認；2，已取消；3，無效；4，退貨；',
        `shipping_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '商品配送情況，0，未發貨；1，已發貨；2，已收貨；3，備貨中',
        `pay_status` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '支付狀態；0，未付款；1，付款中；2，已付款',
        `consignee` varchar(60) NOT NULL COMMENT '收貨人的姓名，使用者頁面填寫，預設取值於表user_address',
        `country` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '收貨人的國家，使用者頁面填寫，預設取值於表user_address，其id對應的值在ecs_region',
        `province` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '收貨人的省份，使用者頁面填寫，預設取值於表user_address，其id對應的值在ecs_region',
        `city` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '收貨人的城市，使用者頁面填寫，預設取值於表user_address，其id對應的值在ecs_region',
        `district` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '收貨人的地區，使用者頁面填寫，預設取值於表user_address，其id對應的值在ecs_region',
        `address` varchar(255) NOT NULL COMMENT '收貨人的詳細位址，使用者頁面填寫，預設取值於表user_address',
        `zipcode` varchar(60) NOT NULL COMMENT '收貨人的郵編，使用者頁面填寫，預設取值於表user_address',
        `tel` varchar(60) NOT NULL COMMENT '收貨人的電話，使用者頁面填寫，預設取值於表user_address',
        `mobile` varchar(60) NOT NULL COMMENT '收貨人的手機，使用者頁面填寫，預設取值於表user_address',
        `email` varchar(60) NOT NULL COMMENT '收貨人的手機，使用者頁面填寫，預設取值於表user_address',
        `best_time` varchar(120) NOT NULL COMMENT '收貨人的最佳送貨時間，使用者頁面填寫，預設取值於表user_address',
        `sign_building` varchar(120) NOT NULL COMMENT '收貨人的位址的標誌性建築，使用者頁面填寫，預設取值於表user_address',
        `postscript` varchar(255) NOT NULL COMMENT '訂單附言，由使用者提交訂單前填寫',
        `shipping_id` tinyint(3) NOT NULL DEFAULT '0' COMMENT '使用者選擇的配送方式id，取值表ecs_shipping',
        `shipping_name` varchar(120) NOT NULL COMMENT '使用者選擇的配送方式的名稱，取值表ecs_shipping',
        `pay_id` tinyint(3) NOT NULL DEFAULT '0' COMMENT '使用者選擇的支付方式的id，取值表ecs_payment',
        `pay_name` varchar(120) NOT NULL COMMENT '使用者選擇的支付方式的名稱，取值表ecs_payment',
        `how_oos` varchar(120) NOT NULL COMMENT '缺貨處理方式，等待所有商品備齊後再發； 取消訂單；與店主協商',
        `how_surplus` varchar(120) NOT NULL COMMENT '根據欄位猜測應該是餘額處理方式，程式未作這部分實現',
        `pack_name` varchar(120) NOT NULL COMMENT '包裝名稱，取值表ecs_pack',
        `card_name` varchar(120) NOT NULL COMMENT '賀卡的名稱，取值ecs_card ',
        `card_message` varchar(255) NOT NULL COMMENT '賀卡內容，由使用者提交',
        `inv_payee` varchar(120) NOT NULL COMMENT '發票抬頭，使用者頁面填寫',
        `inv_content` varchar(120) NOT NULL COMMENT '發票內容，使用者頁面選擇，取值ecs_shop_config的code欄位的值為invoice_content的value',
        `goods_amount` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '商品總金額',
        `shipping_fee` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '配送費用',
        `insure_fee` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '保價費用',
        `pay_fee` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '支付費用,跟支付方式的配置相關，取值表ecs_payment',
        `pack_fee` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '包裝費用，取值表取值表ecs_pack',
        `card_fee` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '賀卡費用，取值ecs_card ',
        `money_paid` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '已付款金額',
        `surplus` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '該訂單使用餘額的數量，取用戶設定餘額，使用者可用餘額，訂單金額中最小者',
        `integral` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用的積分的數量，取用戶使用積分，商品可用積分，用戶擁有積分中最小者',
        `integral_money` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '使用積分金額',
        `bonus` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '使用紅包金額',
        `order_amount` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '應付款金額',
        `from_ad` smallint(5) NOT NULL DEFAULT '0' COMMENT '訂單由某廣告帶來的廣告id，應該取值於ecs_ad',
        `referer` varchar(255) NOT NULL COMMENT '訂單的來源頁面',
        `add_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '訂單生成時間',
        `confirm_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '訂單確認時間',
        `pay_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '訂單支付時間',
        `shipping_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '訂單配送時間',
        `pack_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '包裝id，取值取值表ecs_pack',
        `card_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '賀卡id，使用者在頁面選擇，取值取值ecs_card ',
        `bonus_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '紅包的id，ecs_user_bonus的bonus_id',
        `invoice_no` varchar(50) NOT NULL COMMENT '發貨單號，發貨時填寫，可在訂單查詢查看',
        `extension_code` varchar(30) NOT NULL COMMENT '通過活動購買的商品的代號；GROUP_BUY是團購；AUCTION，是拍賣；SNATCH，奪寶奇兵；正常普通產品該處為空',
        `extension_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '通過活動購買的物品的id，取值ecs_goods_activity；如果是正常普通商品，該處為0',
        `to_buyer` varchar(255) NOT NULL COMMENT '商家給客戶的留言,當該欄位有值時可以在訂單查詢看到',
        `pay_note` varchar(255) NOT NULL COMMENT '付款備註，在訂單管理裡編輯修改',
        `agency_id` smallint(5) unsigned NOT NULL COMMENT '該筆訂單被指派給的辦事處的id，根據訂單內容和辦事處負責範圍自動決定，也可以有管理員修改，取值於表ecs_agency',
        `inv_type` varchar(60) NOT NULL COMMENT '發票類型，使用者頁面選擇，取值ecs_shop_config的code欄位的值為invoice_type的value',
        `tax` decimal(10,2) NOT NULL COMMENT '發票稅額',
        `is_separate` tinyint(1) NOT NULL DEFAULT '0' COMMENT '0，未分成或等待分成；1，已分成；2，取消分成；',
        `parent_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '能獲得推薦分成的用戶id，id取值於表ecs_users',
        `discount` decimal(10,2) NOT NULL COMMENT '折扣金額',
        PRIMARY KEY (`order_id`),
        UNIQUE KEY `order_sn` (`order_sn`),
        KEY `user_id` (`user_id`),
        KEY `order_status` (`order_status`),
        KEY `shipping_status` (`shipping_status`),
        KEY `pay_status` (`pay_status`),
        KEY `shipping_id` (`shipping_id`),
        KEY `pay_id` (`pay_id`),
        KEY `extension_code` (`extension_code`,`extension_id`),
        KEY `agency_id` (`agency_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='訂單的配送，賀卡等詳細資訊' AUTO_INCREMENT=24 ;
```

### <a id='ecs_pack'>ecs_pack</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_pack` (
        `pack_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '包裝配置的自增id',
        `pack_name` varchar(120) NOT NULL COMMENT '包裝的名稱',
        `pack_img` varchar(255) NOT NULL COMMENT '包裝圖紙',
        `pack_fee` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '包裝的費用',
        `free_money` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '訂單達到此金額可以免除該包裝費用',
        `pack_desc` varchar(255) NOT NULL COMMENT '包裝描述',
        PRIMARY KEY (`pack_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品包裝資訊配置表' AUTO_INCREMENT=2 ;
```

### <a id='ecs_package_goods'>ecs_package_goods</a>


```
    CREATE TABLE IF NOT EXISTS `ecs_package_goods` (
      `package_id` mediumint(8) unsigned NOT NULL default '0',
      `goods_id` mediumint(8) unsigned NOT NULL default '0' COMMENT '取值自ecs_goods.goods_id',
      `goods_number` smallint(5) unsigned NOT NULL default '1',
      `admin_id` tinyint(3) unsigned NOT NULL default '0' COMMENT '取值自ecs_admin_user.user_id',
      PRIMARY KEY  (`package_id`,`goods_id`,`admin_id`)
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

好像是2.6.x版後才有的table

### <a id='ecs_payment'>ecs_payment</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_payment` (
        `pay_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '已安裝的支付方式自增id',
        `pay_code` varchar(20) NOT NULL COMMENT '支付方式的英文縮寫，其實就是該支付方式處理外掛程式的不帶尾碼的檔案名部分',
        `pay_name` varchar(120) NOT NULL COMMENT '支付方式名稱',
        `pay_fee` varchar(10) NOT NULL DEFAULT '0' COMMENT '支付費用',
        `pay_desc` text NOT NULL COMMENT '支付方式描述',
        `pay_order` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '支付方式在頁面的顯示順序',
        `pay_config` text NOT NULL COMMENT '支付方式的配置資訊，包括商戶號和金鑰什麼的',
        `enabled` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否可用，0，否；1，是',
        `is_cod` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否貨到付款，0，否；1，是',
        `is_online` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否線上支付，0，否；1，是',
        PRIMARY KEY (`pay_id`),
        UNIQUE KEY `pay_code` (`pay_code`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='安裝的支付方式配置資訊' AUTO_INCREMENT=7 ;
```

### <a id='ecs_pay_log'>ecs_pay_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_pay_log` (
        `log_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '支付記錄自增id',
        `order_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '對應的交易記錄的id，取值表ecs_order_info ',
        `order_amount` decimal(10,2) unsigned NOT NULL COMMENT '支付金額',
        `order_type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '支付類型；0，訂單支付；1，會員預付款支付',
        `is_paid` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否已支付，0，否；1，是',
        PRIMARY KEY (`log_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='系統支付記錄' AUTO_INCREMENT=28 ;
```

### <a id='ecs_plugins'>ecs_plugins</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_plugins` (
        `code` varchar(30) NOT NULL DEFAULT '',
        `version` varchar(10) NOT NULL DEFAULT '',
        `library` varchar(255) NOT NULL DEFAULT '',
        `assign` tinyint(1) unsigned NOT NULL DEFAULT '0',
        `install_date` int(10) unsigned NOT NULL DEFAULT '0',
        PRIMARY KEY (`code`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

尚無資訊，應該是管理plugins用的

### <a id='ecs_region'>ecs_region</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_region` (
        `region_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '表示該地區的id',
        `parent_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '該地區的上一個節點的地區id',
        `region_name` varchar(120) NOT NULL COMMENT '地區的名字',
        `region_type` tinyint(1) NOT NULL DEFAULT '2' COMMENT '該地區的下一個節點的地區id',
        `agency_id` smallint(5) unsigned NOT NULL COMMENT '辦事處的id,這裡有一個bug,同一個省不能有多個辦事處,該欄位只記錄最新的那個辦事處的id',
        PRIMARY KEY (`region_id`),
        KEY `parent_id` (`parent_id`),
        KEY `region_type` (`region_type`),
        KEY `agency_id` (`agency_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='地區清單' AUTO_INCREMENT=419 ;
```

### <a id='ecs_searchengine'>ecs_searchengine</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_searchengine` (
        `date` date NOT NULL DEFAULT '0000-00-00' COMMENT '搜尋引擎訪問日期',
        `searchengine` varchar(20) NOT NULL COMMENT '搜尋引擎名稱',
        `count` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '訪問次數',
        PRIMARY KEY (`date`,`searchengine`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='搜尋引擎訪問記錄';
```

### <a id='ecs_sessions'>ecs_sessions</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_sessions` (
        `sesskey` char(32) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL COMMENT 'sessionid,',
        `expiry` int(10) unsigned NOT NULL DEFAULT '0' COMMENT 'session創建時間',
        `userid` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '如果不是管理員，記錄使用者id',
        `adminid` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '如果是管理員記錄管理員id',
        `ip` char(15) NOT NULL COMMENT '用戶端ip',
        `data` char(255) NOT NULL COMMENT '序列化後的session資料，如果session資料大於255則將資料存到表ecs_sessions_data，此處為空',
        PRIMARY KEY (`sesskey`),
        KEY `expiry` (`expiry`)
        ) ENGINE=MEMORY DEFAULT CHARSET=utf8 COMMENT='session記錄表';
```

the ecs_sessions tables is MEMORY table, it is used to store user
current login information.

### <a id='ecs_sessions_data'>ecs_sessions_data</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_sessions_data` (
        `sesskey` varchar(32) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL COMMENT 'sessionid',
        `expiry` int(10) unsigned NOT NULL DEFAULT '0' COMMENT 'session創建時間',
        `data` longtext NOT NULL COMMENT 'session序列化後的數據',
        PRIMARY KEY (`sesskey`),
        KEY `expiry` (`expiry`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='session資料表（超過255位元組的session內容會保存在該表）';
```

### <a id='ecs_shipping'>ecs_shipping</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_shipping` (
        `shipping_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `shipping_code` varchar(20) NOT NULL COMMENT '配送方式的字串代號',
        `shipping_name` varchar(120) NOT NULL COMMENT '配送方式的名稱',
        `shipping_desc` varchar(255) NOT NULL COMMENT '配送方式的描述',
        `insure` varchar(10) NOT NULL DEFAULT '0' COMMENT '保價費用，單位元，或者是百分數，該值直接輸出為報價費用',
        `support_cod` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否支持貨到付款，1，支持；0，不支持',
        `enabled` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '該配送方式是否被禁用，1，可用；0，禁用',
        PRIMARY KEY (`shipping_id`),
        KEY `shipping_code` (`shipping_code`,`enabled`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='配送方式配置資訊表' AUTO_INCREMENT=9 ;
```

### <a id='ecs_shipping_area'>ecs_shipping_area</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_shipping_area` (
        `shipping_area_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `shipping_area_name` varchar(150) NOT NULL COMMENT '配送方式中的配送區域的名字',
        `shipping_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '該配送區域所屬的配送方式，同ecs_shipping的shipping_id',
        `configure` text NOT NULL COMMENT '序列化的該配送區域的費用配置資訊',
        PRIMARY KEY (`shipping_area_id`),
        KEY `shipping_id` (`shipping_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='配送方式所屬的配送區域和配送費用資訊' AUTO_INCREMENT=9 ;
```

### <a id='ecs_shop_config'>ecs_shop_config</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_shop_config` (
        `id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '全站配置資訊自增id',
        `parent_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '父節點id，取值於該表id欄位的值',
        `code` varchar(30) NOT NULL COMMENT '跟變數名的作用差不多，其實就是語言包中的字串索引，如$_LANG[''cfg_range''][''cart_confirm'']',
        `type` varchar(10) NOT NULL COMMENT '該配置的類型，text，文本輸入框；password，密碼輸入框；textarea，文本區域；select，單選；options，迴圈生成多選；file,文件上傳；manual，手動生成多選；group，是標題分組；hidden，不在頁面顯示',
        `store_range` varchar(255) NOT NULL COMMENT '當語言包中的code欄位對應的是一個陣列時，那該處就是該陣列的索引，如$_LANG[''cfg_range''] [''cart_confirm''][1]；只有type欄位為select,options時才有值',
        `store_dir` varchar(255) NOT NULL COMMENT '當type為file時才有值，檔上傳後的保存目錄',
        `value` text NOT NULL COMMENT '該項配置的值',
        `sort_order` tinyint(3) unsigned NOT NULL DEFAULT '1' COMMENT '顯示順序，數位越大越靠後',
        PRIMARY KEY (`id`),
        UNIQUE KEY `code` (`code`),
        KEY `parent_id` (`parent_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='全站配置資訊表' AUTO_INCREMENT=903 ;
```

### <a id='ecs_snatch_log'>ecs_snatch_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_snatch_log` (
        `log_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `snatch_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '奪寶奇兵活動號，取值於ecs_goods_activity的act_id欄位',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '出價的用戶id，取值於ecs_users的user_id',
        `bid_price` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '出價的價格',
        `bid_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '出價的時間',
        PRIMARY KEY (`log_id`),
        KEY `snatch_id` (`snatch_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=FIXED COMMENT='奪寶奇兵出價記錄表' AUTO_INCREMENT=5 ;
```

### <a id='ecs_stats'>ecs_stats</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_stats` (
        `access_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '存取時間',
        `ip_address` varchar(15) NOT NULL COMMENT '訪問者ip',
        `visit_times` smallint(5) unsigned NOT NULL DEFAULT '1' COMMENT '訪問次數，如果之前有過訪問次數，在以前的基礎上＋1',
        `browser` varchar(60) NOT NULL COMMENT '流覽器及版本',
        `system` varchar(20) NOT NULL COMMENT '作業系統',
        `language` varchar(20) NOT NULL COMMENT '語言',
        `area` varchar(30) NOT NULL COMMENT 'ip所在地區',
        `referer_domain` varchar(100) NOT NULL COMMENT '頁面訪問來源功能變數名稱',
        `referer_path` varchar(200) NOT NULL COMMENT '頁面訪問來源除功能變數名稱外的路徑部分',
        `access_url` varchar(255) NOT NULL COMMENT '訪問分頁檔名',
        KEY `access_time` (`access_time`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='訪問資訊記錄表';
```

### <a id='ecs_tag'>ecs_tag</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_tag` (
        `tag_id` mediumint(8) NOT NULL AUTO_INCREMENT COMMENT '商品標籤自增id',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '用戶的id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '商品的id',
        `tag_words` varchar(255) NOT NULL COMMENT '標籤內容',
        PRIMARY KEY (`tag_id`),
        KEY `user_id` (`user_id`),
        KEY `goods_id` (`goods_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='商品的標記' AUTO_INCREMENT=3 ;
```

### <a id='ecs_template'>ecs_template</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_template` (
        `filename` varchar(30) NOT NULL COMMENT '該條範本配置屬於哪個範本頁面',
        `region` varchar(40) NOT NULL COMMENT '該條範本配置在它所屬的範本檔中的位置',
        `library` varchar(40) NOT NULL COMMENT '該條範本配置在它所屬的範本檔中的位置處應該引入的lib的相對目錄位址',
        `sort_order` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '範本檔中這個位置的引入lib項的值的顯示順序',
        `id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '欄位意義待查',
        `number` tinyint(1) unsigned NOT NULL DEFAULT '5' COMMENT '每次顯示多少個值',
        `type` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '屬於哪個動態項，0，固定項；1，分類下的商品；2，品牌下的商品；3，文章列表；4，廣告位 ',
        `theme` varchar(60) NOT NULL COMMENT '該範本配置項屬於哪套範本的範本名',
        `remarks` varchar(30) NOT NULL COMMENT '備註，可能是預留欄位，沒有值所以沒確定用途',
        KEY `filename` (`filename`,`region`),
        KEY `theme` (`theme`),
        KEY `remarks` (`remarks`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='範本設置資料表';
```

### <a id='ecs_topic'>ecs_topic</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_topic` (
        `topic_id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '專題自增id',
        `title` varchar(255) NOT NULL DEFAULT '''''' COMMENT '專題名稱',
        `intro` text NOT NULL COMMENT '專題介紹',
        `start_time` int(11) NOT NULL DEFAULT '0' COMMENT '專題開始時間',
        `end_time` int(10) NOT NULL DEFAULT '0' COMMENT '結束時間',
        `data` text NOT NULL COMMENT '專題資料內容，包括分類，商品等',
        `template` varchar(255) NOT NULL DEFAULT '''''' COMMENT '專題範本檔',
        `css` text NOT NULL COMMENT '專題樣式代碼',
        KEY `topic_id` (`topic_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='專題活動配置表' AUTO_INCREMENT=2 ;
```

### <a id='ecs_users'>ecs_users</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_users` (
        `user_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '會員資料自增id',
        `email` varchar(60) NOT NULL COMMENT '會員郵箱',
        `user_name` varchar(60) NOT NULL COMMENT '用戶名',
        `password` varchar(32) NOT NULL COMMENT '使用者密碼',
        `question` varchar(255) NOT NULL COMMENT '安全問題答案',
        `answer` varchar(255) NOT NULL COMMENT '安全問題',
        `sex` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '性別，0，保密；1，男；2，女',
        `birthday` date NOT NULL DEFAULT '0000-00-00' COMMENT '生日日期',
        `user_money` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '用戶現有資金',
        `frozen_money` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '用戶凍結資金',
        `pay_points` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '消費積分',
        `rank_points` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '會員等級積分',
        `address_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '預設的收貨信息地址id，取值表ecs_user_address ',
        `reg_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '註冊時間',
        `last_login` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '最後一次登錄時間',
        `last_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '應該是最後一次修改資訊時間，該表資訊從其他表同步過來考慮',
        `last_ip` varchar(15) NOT NULL COMMENT '最後一次登錄ip',
        `visit_count` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '登錄次數',
        `user_rank` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '會員登記id，取值ecs_user_rank',
        `is_special` tinyint(3) unsigned NOT NULL DEFAULT '0',
        `salt` varchar(10) NOT NULL DEFAULT '0',
        `parent_id` mediumint(9) NOT NULL DEFAULT '0' COMMENT '推薦人會員id，',
        `flag` tinyint(3) unsigned NOT NULL DEFAULT '0',
        `alias` varchar(60) NOT NULL COMMENT '昵稱',
        `msn` varchar(60) NOT NULL COMMENT 'msn',
        `qq` varchar(20) NOT NULL COMMENT 'qq號',
        `office_phone` varchar(20) NOT NULL COMMENT '辦公電話',
        `home_phone` varchar(20) NOT NULL COMMENT '家庭電話',
        `mobile_phone` varchar(20) NOT NULL COMMENT '手機',
        `is_validated` tinyint(3) unsigned NOT NULL DEFAULT '0',
        `credit_line` decimal(10,2) unsigned NOT NULL COMMENT '信用額度，目前2.6.0版好像沒有作實現',
        PRIMARY KEY (`user_id`),
        UNIQUE KEY `user_name` (`user_name`),
        KEY `email` (`email`),
        KEY `parent_id` (`parent_id`),
        KEY `flag` (`flag`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC AUTO_INCREMENT=21 ;
```

### <a id='ecs_user_account'>ecs_user_account</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_user_account` (
        `id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增ID號',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '用戶登錄後保存在session中的id號，跟users表中的user_id對應',
        `admin_user` varchar(255) NOT NULL COMMENT '操作該筆交易的管理員的用戶名',
        `amount` decimal(10,2) NOT NULL COMMENT '資金的數目，正數為增加，負數為減少',
        `add_time` int(10) NOT NULL DEFAULT '0' COMMENT '記錄插入時間',
        `paid_time` int(10) NOT NULL DEFAULT '0' COMMENT '記錄更新時間',
        `admin_note` varchar(255) NOT NULL COMMENT '管理員的被准',
        `user_note` varchar(255) NOT NULL COMMENT '用戶的被准',
        `process_type` tinyint(1) NOT NULL DEFAULT '0' COMMENT '操作類型，1，退款；0，預付費，其實就是充值',
        `payment` varchar(90) NOT NULL COMMENT '支付管道的名稱，取自payment的pay_name欄位',
        `is_paid` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否已經付款，0，未付；1，已付',
        PRIMARY KEY (`id`),
        KEY `user_id` (`user_id`),
        KEY `is_paid` (`is_paid`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='用戶資金流動表，包括提現和充值' AUTO_INCREMENT=7 ;
```

### <a id='ecs_user_address'>ecs_user_address</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_user_address` (
        `address_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
        `address_name` varchar(50) NOT NULL,
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '用戶表中的流水號',
        `consignee` varchar(60) NOT NULL COMMENT '收貨人的名字',
        `email` varchar(60) NOT NULL COMMENT '收貨人的email',
        `country` smallint(5) NOT NULL DEFAULT '0' COMMENT '收貨人的國家',
        `province` smallint(5) NOT NULL DEFAULT '0' COMMENT '收貨人的省份',
        `city` smallint(5) NOT NULL DEFAULT '0' COMMENT '收貨人的城市',
        `district` smallint(5) NOT NULL DEFAULT '0' COMMENT '收貨人的地區',
        `address` varchar(120) NOT NULL COMMENT '收貨人的詳細地址',
        `zipcode` varchar(60) NOT NULL COMMENT '收貨人的郵編',
        `tel` varchar(60) NOT NULL COMMENT '收貨人的電話',
        `mobile` varchar(60) NOT NULL COMMENT '收貨人的手機',
        `sign_building` varchar(120) NOT NULL COMMENT '收貨位址的標誌性建築名',
        `best_time` varchar(120) NOT NULL COMMENT '收貨人的最佳收貨時間',
        PRIMARY KEY (`address_id`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='收貨人的信息表' AUTO_INCREMENT=4 ;
```

在ecs_user_address的user_id與ec_users
table形成多對一的關聯(一個user可以有多個收貨地址)，表示一個人
但ecs_users中也有一個address_id(預設的收貨地址)，則跟ecs_user_address的address_id形成一對一的關聯。

### <a id='ecs_user_bonus'>ecs_user_bonus</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_user_bonus` (
        `bonus_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '紅包的流水號',
        `bonus_type_id` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '紅包發送類型.0,按使用者如會員等級,會員名稱發放;1,按商品類別發送;2,按訂單金額所達到的額度發送;3,線下發送',
        `bonus_sn` bigint(20) unsigned NOT NULL DEFAULT '0' COMMENT '紅包號,如果為0就是沒有紅包號.如果大於0,就需要輸入該紅包號才能使用紅包',
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該紅包屬於某會員的id.如果為0,就是該紅包不屬於某會員',
        `used_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '紅包使用的時間',
        `order_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '使用了該紅包的交易號',
        `emailed` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '猜的，應該是是否已經將紅包發送到用戶的郵箱；1，是；0，否；',
        PRIMARY KEY (`bonus_id`),
        KEY `user_id` (`user_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 COMMENT='已經發送的紅包資訊清單' AUTO_INCREMENT=122 ;
```

ecs_user_bonus.user_id :
該紅包屬於某會員的id.**如果為0,就是該紅包不屬於某會員**,

### <a id='ecs_user_feed'>ecs_user_feed</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_user_feed` (
        `feed_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,
        `user_id` mediumint(8) unsigned NOT NULL DEFAULT '0',
        `value_id` mediumint(8) unsigned NOT NULL DEFAULT '0',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0',
        `feed_type` tinyint(1) unsigned NOT NULL DEFAULT '0',
        `is_feed` tinyint(1) unsigned NOT NULL DEFAULT '0',
        PRIMARY KEY (`feed_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 AUTO_INCREMENT=1 ;
```

there is no information about ecs_user_feed now.

### <a id='ecs_user_rank'>ecs_user_rank</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_user_rank` (
        `rank_id` tinyint(3) unsigned NOT NULL AUTO_INCREMENT COMMENT '會員等級編號，其中0是非會員',
        `rank_name` varchar(30) NOT NULL COMMENT '會員等級名稱',
        `min_points` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '該等級的最低積分',
        `max_points` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '該等級的最高積分',
        `discount` tinyint(3) unsigned NOT NULL DEFAULT '0' COMMENT '該會員等級的商品折扣',
        `show_price` tinyint(1) unsigned NOT NULL DEFAULT '1' COMMENT '是否在不是該等級會員購買頁面顯示該會員等級的折扣價格.1,顯示;0,不顯示',
        `special_rank` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '是否事特殊會員等級組.0,不是;1,是',
        PRIMARY KEY (`rank_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='會員等級配置資訊' AUTO_INCREMENT=3 ;
```

### <a id='ecs_virtual_card'>ecs_virtual_card</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_virtual_card` (
        `card_id` mediumint(8) NOT NULL AUTO_INCREMENT COMMENT '虛擬卡卡號自增id',
        `goods_id` mediumint(8) unsigned NOT NULL DEFAULT '0' COMMENT '該虛擬卡對應的商品id，取值於表ecs_goods',
        `card_sn` varchar(60) NOT NULL COMMENT '加密後的卡號',
        `card_password` varchar(60) NOT NULL COMMENT '加密後的密碼',
        `add_date` int(11) NOT NULL DEFAULT '0' COMMENT '卡號添加日期',
        `end_date` int(11) NOT NULL DEFAULT '0' COMMENT '卡號截至使用日期',
        `is_saled` tinyint(1) NOT NULL DEFAULT '0' COMMENT '是否賣出，0，否；1，是',
        `order_sn` varchar(20) NOT NULL COMMENT '賣出該卡號的交易號，取值表ecs_order_info',
        `crc32` int(11) NOT NULL DEFAULT '0' COMMENT 'crc32後的key',
        PRIMARY KEY (`card_id`),
        KEY `goods_id` (`goods_id`),
        KEY `car_sn` (`card_sn`),
        KEY `is_saled` (`is_saled`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='虛擬卡卡號庫' AUTO_INCREMENT=8 ;
```

### <a id='ecs_volume_price'>ecs_volume_price</a>


```
    CREATE TABLE IF NOT EXISTS `ecs_volume_price` (
      `price_type` tinyint(1) unsigned NOT NULL,
      `goods_id` mediumint(8) unsigned NOT NULL COMMENT '取值自ecs_goods.goods_id',
      `volume_number` smallint(5) unsigned NOT NULL default '0',
      `volume_price` decimal(10,2) NOT NULL default '0.00',
      PRIMARY KEY  (`price_type`,`goods_id`,`volume_number`)
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
```

尚無資訊

### <a id='ecs_vote'>ecs_vote</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_vote` (
        `vote_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '線上調查自增id',
        `vote_name` varchar(250) NOT NULL COMMENT '線上調查主題',
        `start_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '線上調查開始時間',
        `end_time` int(11) unsigned NOT NULL DEFAULT '0' COMMENT '線上調查結束時間',
        `can_multi` tinyint(1) unsigned NOT NULL DEFAULT '0' COMMENT '能否多選，0，可以；1，不可以',
        `vote_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '投票人數也可以說投票次數',
        PRIMARY KEY (`vote_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='網站調查資訊記錄表' AUTO_INCREMENT=3 ;
```

### <a id='ecs_vote_log'>ecs_vote_log</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_vote_log` (
        `log_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '投票記錄自增id',
        `vote_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '關聯的投票主題id，取值表ecs_vote',
        `ip_address` varchar(15) NOT NULL COMMENT '投票的ip地址',
        `vote_time` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '投票的時間',
        PRIMARY KEY (`log_id`),
        KEY `vote_id` (`vote_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='投票記錄表' AUTO_INCREMENT=5 ;
```

### <a id='ecs_vote_option'>ecs_vote_option</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_vote_option` (
        `option_id` smallint(5) unsigned NOT NULL AUTO_INCREMENT COMMENT '投票選項自增id',
        `vote_id` smallint(5) unsigned NOT NULL DEFAULT '0' COMMENT '關聯的投票主題id，取值表ecs_vote',
        `option_name` varchar(250) NOT NULL COMMENT '投票選項的值',
        `option_count` int(8) unsigned NOT NULL DEFAULT '0' COMMENT '該選項的票數',
        PRIMARY KEY (`option_id`),
        KEY `vote_id` (`vote_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='投票的選項內容表' AUTO_INCREMENT=8 ;
```

### <a id='ecs_wholesale'>ecs_wholesale</a>


```
      CREATE TABLE IF NOT EXISTS `ecs_wholesale` (
        `act_id` mediumint(8) unsigned NOT NULL AUTO_INCREMENT COMMENT '批發方案自增id',
        `goods_id` mediumint(8) unsigned NOT NULL COMMENT '商品id',
        `goods_name` varchar(255) NOT NULL COMMENT '商品名稱',
        `rank_ids` varchar(255) NOT NULL COMMENT '適用會員登記，多個值之間用逗號分隔，取值於ecs_user_rank',
        `prices` text NOT NULL COMMENT '序列化後的商品屬性，數量，價格',
        `enabled` tinyint(3) unsigned NOT NULL COMMENT '批發方案是否可用',
        PRIMARY KEY (`act_id`),
        KEY `goods_id` (`goods_id`)
        ) ENGINE=MyISAM DEFAULT CHARSET=utf8 ROW_FORMAT=DYNAMIC COMMENT='批發方案表' AUTO_INCREMENT=3 ;
```

