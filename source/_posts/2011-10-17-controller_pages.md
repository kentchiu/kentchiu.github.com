---
author: Kent Chiu
published: true
layout: post
title: "Controllers"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - ecshop
---





1.  activity.php - 活動列表
2.  affiche.php - 廣告處理檔
3.  affiliate.php - 生成商品列表
4.  article\_cat.php - 文章內容
5.  article.php - 文章分類
6.  auction.php - 拍賣前臺文件
7.  brand.php - 品牌列表
8.  captcha.php - 生成驗證碼
9.  catalog.php - 列出所以分類及品牌
10. category.php - 商品分類
11. comment.php - 提交用戶評論
12. compare.php - 商品比較程式
13. cycle\_image.php - 輪播圖片程式
14. exchange.php - 積分商城
15. feed.php - 生成程式
16. flow.php - 購物流程
17. gallery.php - 商品相冊
18. goods\_script.php - 生成商品列表
19. goods.php - 商品詳情
20. group\_buy.php - 團購商品前臺檔
21. index.php - 首頁文件
22. message.php - 留言板
23. myship.php - 支付配送DEMO
24. pick\_out.php - 選購中心
25. pm.php - 短消息檔
26. quotation.php - 報價單
27. receive.php - 處理收回確認的頁面
28. region.php - 地區切換程式
29. respond.php - 支付回應頁面
30. search.php - 搜索程式
31. sitemaps.php - google sitemap 文件
32. snatch.php - 奪寶奇兵前臺頁面
33. tag\_cloud.php - 標籤雲
34. topic.php - 專題前臺
35. user.php - 會員中心
36. vote.php - 調查程式
37. wholesale.php - 批發前臺文件

activity.php
------------

affiche.php
-----------

affiliate.php
-------------

article\_cat.php
----------------

article.php
-----------

auction.php
-----------

brand.php
---------

captcha.php
-----------

catalog.php
-----------

category.php
------------

comment.php
-----------

compare.php
-----------

cycle\_image.php
----------------

exchange.php
------------

feed.php
--------

flow.php
--------

gallery.php
-----------

goods\_script.php
-----------------

goods.php
---------

group\_buy.php
--------------

index.php
---------

message.php
-----------

myship.php
----------

pick\_out.php
-------------

pm.php
------

quotation.php
-------------

receive.php
-----------

region.php
----------

respond.php
-----------

search.php
----------

sitemaps.php
------------

snatch.php
----------

tag\_cloud.php
--------------

topic.php
---------

user.php
--------

本頁是會員中心，專門處理一些跟會員有關的動作，動作是由action
parameter，action parameter可能的值如下：

1.  account\_deposit -
2.  account\_detail -
3.  add\_booking - 添加缺貨登記頁面
4.  add\_tag - 添加標籤(ajax)
5.  add\_to\_attention - 添加關注商品
6.  address\_list - 收貨地址清單介面
7.  account\_deposit - 會員預付款介面
8.  account\_detail - 會員帳目明細介面
9.  account\_log - 會員充值和提現申請記錄
10. account\_raply -會員退款申請介面
11. act\_account - 對會員餘額申請的處理
12. act\_add\_bonus - 添加一個紅包
13. act\_add\_booking - 添加缺貨登記的處理
14. act\_add\_message - 添加我的留言
15. act\_del\_booking - 刪除缺貨登記
16. act\_edit\_address - 添加/編輯收貨位址的處理
17. act\_edit\_password - 修改會員密碼
18. act\_edit\_payment - 編輯使用餘額支付的處理
19. act\_edit\_profile - 修改個人資料的處理
20. act\_edit\_surplus - 編輯使用餘額支付的處理
21. act\_login - 處理會員的登錄
22. act\_register - 註冊會員的處理
23. affiliate - 使用者推薦頁面
24. affirm\_received - 確認收貨
25. bonus - 我的紅包列表
26. booking\_list - 顯示缺貨登記清單
27. cancel - 刪除會員餘額
28. cancel\_order - 取消訂單
29. check\_email- 驗證用戶郵箱地址是否被註冊
30. collection\_list - 顯示收藏商品清單
31. collect - 添加收藏商品(ajax)
32. comment\_list - 顯示評論清單
33. default - 用戶中心歡迎頁
34. del\_attention - 取消關注商品
35. del\_cmt - 刪除評論
36. delete\_collection - 刪除收藏的商品
37. del\_msg - 刪除留言
38. drop\_consignee - 刪除收貨地址
39. get\_password - 密碼找回–\>修改密碼介面
40. email\_list - 首頁郵件訂閱ajax操做和驗證操作
41. group\_buy - 我的團購列表
42. group\_buy\_detail - 團購訂單詳情
43. is\_registered - 驗證用戶註冊用戶名是否已註冊
44. login - 使用者登錄介面
45. logout - 退出會員中心
46. message\_list - 顯示留言清單
47. merge\_order - 合併訂單
48. order\_detail - 查看訂單詳情
49. order\_list - 查看訂單清單
50. order\_query -
51. pay - 會員通過帳目明細列表進行再付款的操作
52. password -
53. profile - 個人資料頁面
54. register - 顯示會員註冊介面
55. reset\_password - 重置新密碼
56. return\_to\_cart - 將指定訂單中商品添加到購物車
57. save\_order\_address - 保存訂單詳情收貨位址
58. send\_hash\_mail - ajax 發送驗證郵件
59. send\_pwd\_email - 發送密碼修改確認郵件
60. signin - 處理 ajax 的登錄請求 (又是一個名字對不起來的)
61. tag\_list - 標籤雲列表
62. track\_packages -
63. transform\_points -
64. validate\_email - 驗證用戶註冊郵件

vote.php
--------

wholesale.php
-------------

