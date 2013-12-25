---
published: true
author: Kent Chiu
layout: post
title: "使用curl指令測試REST服務"
date: 2013-08-14 11:26
comments: true
sharing: true
footer: true
categories: 
- curl
- bash
- command
- rest
---

[cURL](http://en.wikipedia.org/wiki/CURL) 是很方便的Rest客戶端，可以很方便的完成許多Rest API測試的需求，甚至，如果是需要先登入或認證的rest api，也可以進行測試，利用*curl*指令，可以送出HTTP GET, POST, PUT, DELETE, 也可以改變 HTTP header來滿足使用REST API需要的特定條件。

[curl的參數很多](http://curl.haxx.se/docs/manpage.html)，這邊僅列出目前測試REST時常用到的:
	
	-X/--request [GET|POST|PUT|DELETE|…]  使用指定的http method發出 http request
	-H/--header                           設定request裡的header
	-i/--include                          顯示response的header
	-d/--data                             設定 http parameters 
	-v/--verbose                          輸出比較多的訊息
	-u/--user                             使用者帳號、密碼
	-b/--cookie                           cookie  

> linux command line 的參數常，同一個功能常會有兩個功能完全相同參數，一個是比較短的參數，前面通常是用`-`(一個`-`)導引符號，另一個比較長的參數，通常會用`--`(兩個`-`)導引符號
>
> 在curl 使用說明
> 
>  		-X, --request COMMAND  Specify request command to use
>     		--resolve HOST:PORT:ADDRESS  Force resolve of HOST:PORT to ADDRESS
>     		--retry NUM   Retry request NUM times if transient problems occur
>     		--retry-delay SECONDS When retrying, wait this many seconds between each
>     		--retry-max-time SECONDS  Retry only within this period>
>
> 參數`-X`跟`--request`兩個功能是一樣的，所以使用時 
> `ex:curl -X POST http://www.example.com/` 跟 `curl --request POST http://www.example.com/` 是相等的功能


#### GET/POST/PUT/DELETE使用方式 ####
-X 後面加 http method，

	curl -X GET "http://www.rest.com/api/users"
	curl -X POST "http://www.rest.com/api/users"
	curl -X PUT "http://www.rest.com/api/users"
	curl -X DELETE "http://www.rest.com/api/users"

url要加引號也可以，不加引號也可以，如果有非純英文字或數字外的字元，不加引號可能會有問題，如果是網碼過的url，也要加上引號

#### HEADER ####
在http header加入的訊息

	curl -v -i -H "Content-Type: application/json" http://www.example.com/users

#### HTTP Parameter ####
http參數可以直接加在url的query string，也可以用`-d`帶入參數間用`&`串接


	curl -X POST -d "param1=value1&param2=value2"
	curl -X POST -d "param1=a 0space"     
	# "a space" url encode後空白字元會編碼成'%20'為"a%20space"，編碼後的參數可以直接使用
	curl -X POST -d "param1=a%20space"     

#### post json 格式得資料 ####
如同時需要傳送request parameter跟json，request parameter可以加在url後面，json資料則放入`-d`的參數，然後利用單引號將json資料含起來(如果json內容是用單引號，-d的參數則改用雙引號包覆)，header要加入"Content-Type:application/json"跟"Accept:application/json"


	curl http://www.example.com?modifier=kent -X PUT -i -H "Content-Type:application/json" -H "Accept:application/json" -d '{"boolean" : false, "foo" : "bar"}'
	# 不加"Accept:application/json"也可以
	curl http://www.example.com?modifier=kent -X PUT -i -H "Content-Type:application/json" -d '{"boolean" : false, "foo" : "bar"}'
	

#### 需先認證或登入才能使用的service #####	
許多服務，需先進行登入或認證後，才能存取其API服務，依服務要求的條件，的curl可以透過cookie，session或加入在header加入session key，api key或認證的token來達到認證的效果。

session 例子: 

後端如果是用session記錄使用者登入資訊，後端會傳一個 session id給前端，前端需要在每次跟後端的requests的header中置入此session id，後端便會以此session id識別前端是屬於那個session，以達到session的效果

	curl --request GET 'http://www.rest.com/api/users' --header 'sessionid:1234567890987654321'

cookie 例子

如果是使用cookie，在認證後，後端會回一個cookie回來，把該cookie成檔案，當要存取需要任務的url時，再用`-b cookie_file` 的方式在request中植入cookie即可正常使用
	
	# 將cookie存檔
	curl -i -X POST -d username=kent -d password=kent123 -c  ~/cookie.txt  http://www.rest.com/auth
	# 載入cookie到request中	
	curl -i --header "Accept:application/json" -X GET -b ~/cookie.txt http://www.rest.com/users/1
	
#### 檔案上傳

	curl -i -X POST -F 'file=@/Users/kent/my_file.txt' -F 'name=a_file_name'
	
這個是透過 HTTP multipart POST 上傳資料， `-F` 是使用http query parameter的方式，指定檔案位置的參數要加上`@` 		

	
相關資源	
-------
- <http://linux.about.com/od/commands/l/blcmdl1_curl.htm> - curl 手冊	