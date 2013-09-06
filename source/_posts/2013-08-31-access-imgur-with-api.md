---
published: true
author: Kent Chiu
layout: post
title: "Access imgur with API"
date: 2013-08-31 01:20
comments: true
sharing: true
footer: true
categories: 
- rest
- imgur
---


1. 	先註冊 [imgur](http://imgur.com/) 的帳號
2. 	申請API使用權限
   	![2013-08-31-access-imgur-with-api-001.png][]
3. 	註冊成後功，imgur會寄信到email裡，裡面會有 client_id 跟 client_secret 
4. 	試車 `https://api.imgur.com/3/gallery.json` 是取得

		curl https://api.imgur.com/3/gallery.json -i -H "Authorization: Client-ID 69a8cxxxxxxxxxx" 
		
		HTTP/1.1 200 OK
		Server: nginx
		Date: Fri, 30 Aug 2013 17:59:29 GMT
		Content-Type: application/json
		Transfer-Encoding: chunked
		Connection: keep-alive
		…..

	如果看到	http status code 200，就表示成功了
	
	> 69a8cxxxxxxxxxx -> 換成你自已的	client_id
	
#### 取得相簿 ####


	curl -i -H "Authorization: Client-ID 69a8cxxxxxxxxxx"  https://api.imgur.com/3/gallery/album/lDRB2/json
		https://api.imgur.com/3/gallery/album/lDRB2/json

	{
	    "data": {
	        "id": "lDRB2",
	        "title": "Imgur Office",
	        "description": null,
	        "datetime": 1357856292,
	        "cover": "24nLu",
	        "account_url": "Alan",
	        "privacy": "public",
	        "layout": "blog",
	        "views": 13780,
	        "link": "http://alanbox.imgur.com/a/lDRB2",
	        "ups": 1602,
	        "downs": 14,
	        "score": 1917,
	        "is_album": true,
	        "vote": null,
	        "images_count": 11,
	        "images": [
	            {
	                "id": "24nLu",
	                "title": null,
	                "description": null,
	                "datetime": 1357856352,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 855658,
	                "views": 135772,
	                "bandwidth": 116174397976,
	                "link": "http://i.imgur.com/24nLu.jpg"
	            },
	            {
	                "id": "Ziz25",
	                "title": null,
	                "description": null,
	                "datetime": 1357856394,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 919391,
	                "views": 135493,
	                "bandwidth": 124571044763,
	                "link": "http://i.imgur.com/Ziz25.jpg"
	            },
	            {
	                "id": "9tzW6",
	                "title": null,
	                "description": null,
	                "datetime": 1357856385,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 655028,
	                "views": 135063,
	                "bandwidth": 88470046764,
	                "link": "http://i.imgur.com/9tzW6.jpg"
	            },
	            {
	                "id": "dFg5u",
	                "title": null,
	                "description": null,
	                "datetime": 1357856378,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 812738,
	                "views": 134704,
	                "bandwidth": 109479059552,
	                "link": "http://i.imgur.com/dFg5u.jpg"
	            },
	            {
	                "id": "oknLx",
	                "title": null,
	                "description": null,
	                "datetime": 1357856338,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 1749,
	                "height": 2332,
	                "size": 717324,
	                "views": 32938,
	                "bandwidth": 23627217912,
	                "link": "http://i.imgur.com/oknLx.jpg"
	            },
	            {
	                "id": "OL6tC",
	                "title": null,
	                "description": null,
	                "datetime": 1357856321,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 1443262,
	                "views": 32346,
	                "bandwidth": 46683752652,
	                "link": "http://i.imgur.com/OL6tC.jpg"
	            },
	            {
	                "id": "cJ9cm",
	                "title": null,
	                "description": null,
	                "datetime": 1357856330,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 544702,
	                "views": 31829,
	                "bandwidth": 17337319958,
	                "link": "http://i.imgur.com/cJ9cm.jpg"
	            },
	            {
	                "id": "7BtPN",
	                "title": null,
	                "description": null,
	                "datetime": 1357856369,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 844863,
	                "views": 31257,
	                "bandwidth": 26407882791,
	                "link": "http://i.imgur.com/7BtPN.jpg"
	            },
	            {
	                "id": "42ib8",
	                "title": null,
	                "description": null,
	                "datetime": 1357856424,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 2592,
	                "height": 1944,
	                "size": 905073,
	                "views": 30945,
	                "bandwidth": 28007483985,
	                "link": "http://i.imgur.com/42ib8.jpg"
	            },
	            {
	                "id": "BbwIx",
	                "title": null,
	                "description": null,
	                "datetime": 1357856360,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 1749,
	                "height": 2332,
	                "size": 662413,
	                "views": 30107,
	                "bandwidth": 19943268191,
	                "link": "http://i.imgur.com/BbwIx.jpg"
	            },
	            {
	                "id": "x7b91",
	                "title": null,
	                "description": null,
	                "datetime": 1357856406,
	                "type": "image/jpeg",
	                "animated": false,
	                "width": 1944,
	                "height": 2592,
	                "size": 618567,
	                "views": 29259,
	                "bandwidth": 18098651853,
	                "link": "http://i.imgur.com/x7b91.jpg"
	            }
	        ]
	    },
	    "success": true,
	    "status": 200
	}				

	
[2013-08-31-access-imgur-with-api-001.png]:http://blog.kent-chiu.com/images/2013-08-31/2013-08-31-access-imgur-with-api-001.png	




