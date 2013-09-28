---
published: true
author: Kent Chiu
layout: post
title: "RESTful API Design"
date: 2013-06-04 09:58
comments: true
sharing: true
footer: true
categories: 
- rest 
---


## Method


POST,DELETE : 操作是 non-idempotent(非幕等)

GET, PUT  : 操作是 idempotent(幕等)

> TBC : 解釋 idempotent

### GET

GET method用來取得一筆或多的資源，如果是多筆資源，還可以加入分頁，過濾等資訊，也可在header傳入分頁的links，ex:'first', 'last', 'next' and 'prev'

* 	GET 	http://www.example.com/orders			  				取得多筆訂單
* 	GET 	http://www.example.com/orders/12345       				取得訂單編號為 12345 的訂單
* 	GET 	http://www.example.com/orders/12345/items      			取得訂單編號為 12345 的訂單下的所有訂單項目
* 	GET 	http://www.example.com/orders/12345/oitems/678      	取得訂單編號為 12345 的訂單下的訂單項目 678
* 	GET 	http://www.example.com/orders?customer=kent    			取得客戶 kent 的所有訂單

##### status code

*	200 (OK) 			順利取得資源
*	400 (BAD REQUEST) 	無法順利取得資訊，通常是參數有問題或某個查詢條件失效
*	404 (Not Found) 	資源不存在


### POST

POST method用來建立新資源，建立完成資源後，通常是回應 201(CREATED) 的狀態碼，而且建立的新資源的 uri link 會放在 HEAD (不是response body)


	POST http://www.example.com/order


response
	200 OK
	Content-Type: application/json
	Location: http://www.example.com/order/123	


> POST, PUT, PATCH 出去的資料應該儘量採用json，而不是Request Parameter(form submit)的格式
> 而且header必須加上 `application/json` 否則就要丟出 HTTP 405 Unsupported Media Type的error

##### status code

*	201 (CREATED) 		成功建立新資源
*	404 (Not Found) 	資源不存在


### PUT
POST method 用來更新資源，

如果 resource 的 id，是由前端決定，而不是後端，那麼此時 PUT 也可以拿用做建立新資源的動作

##### status code

*   200 (OK) 			更新成功，如果使用這個status code，response裡會有異動後的內容
*	201 (CREATED) 		成功建立新資源
* 	204 (No Content)    更新成功，如果使用這個status code，response裡沒有任何內容
*	404 (Not Found) 	資源不存在



### DELETE





##### status code

*   200 (OK) 			更新成功，如果使用這個 status code，response裡會有異動後的內容
* 	204 (No Content)    更新成功，如果使用這個 status code，response裡沒有任何內容
*	404 (Not Found) 	資源不存在，連續呼叫兩次相同的 delete，會傳回 404


#### 命名規則

資源命名應為**複數名詞**，不論是 GET, POST, PUT, DELETE 應該都要用**複數名詞**來命名，如果是要取得單筆資訊，
則是在**複數名詞**的資源後接上該資源的indentity

如果是複合字，應該用`-`隔開，而不是用 camel style : ex: 採用 `hello-world` 而非 `helloWorld`


* 	GET 	http://www.example.com/orders			  取得多筆訂單
* 	GET 	http://www.example.com/orders/12345       取得訂單編號為 12345 的訂單
* 	GET 	http://www.example.com/users?name=kent    取得使用者 kent 的資訊
*  	PUT 	http://www.example.com/users/kent         更新使用者 kent
*   POST 	http://www.example.com/users/kent         新增使用者 kent
*   DELETE	http://www.example.com/users/kent         刪除使用者 kent

所以，一般來說，只有兩種 url 的定義方式

	GET | PUT | DELETE 	http://www.example.com/orders/{id}
	POST 				http://www.example.com/orders

> TBC : 查一下 resource id是用純數字(/user/1)，或使用有意義的名稱為佳(users/kent)
> 目前認為用 id 應該會比較好，因為名稱可能會異動，如果要用名稱，應該是類似查詢參數的用法 /users?name=kent

#### 輔助用字
- search 搜尋，如果有時就是做搜尋當resource最直覺，就用吧，以名詞命規的規格，還是可以有例外的
- filter 過濾用
- page   分頁用
- sort   排序用，可以用`-`表示昇冪, ex: sort=-age 由大到小排序，sort=age 由小到大排序
- fields 用來指定後端只傳合那些欄位  ex: fields=id,name,address
- embed  用來指定後端傳合的部份是不是包含 detail，有些資料是master/detail的關係，用embed可以決定要不要傳回 detail

## Resource

- 	<http://zh.wikipedia.org/zh-tw/HTTP%E7%8A%B6%E6%80%81%E7%A0%81> - HTTP 狀態碼
-	<http://www.restapitutorial.com/lessons/httpmethods.html>  - RESTful Tutorial
-	<http://blog.2partsmagic.com/restful-uri-design/> - rest 命名規格
-	<http://stackoverflow.com/questions/1619152/how-to-create-rest-urls-without-verbs> - 如何避免用動詞命名
- 	一般流行的 Rest API
	1.   <http://docs.aws.amazon.com/AmazonS3/latest/API/APIRest.html> - Amazon 的 REST API文件
	2.   <https://dev.twitter.com/docs/api/1.1/get/lists/list> - twitter 的 REST API文件
	3.   <https://developers.facebook.com/docs/reference/api/> - FaceBook 的 REST API文件
	4.   <https://developer.linkedin.com/apis> - linkedin 的 REST API文件
	5.   <https://developer.paypal.com/webapps/developer/docs/api/> - paypal  的 REST API文件
- 	<http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api?hn#snake-vs-camel> - 設計Restful API 相當不錯的參考資料	，內容很全面，方方面面都有提到