---
published: true
author: Kent Chiu
layout: post
title: "Restful Web Security"
date: 2013-07-29 10:36
comments: true
sharing: true
footer: true
categories: 
- rest
- authentication
- authorization
- security
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------


Restful Web Service 常用的保全方式有：

1. session and cookies
2. HTTP 標準認證/摘要驗證(digest authentication)
3. API key

各有各的優缺點:
採用 session 記錄 user 的認證的方式，有違 rest stateless 的特性, cookies 只有 http 能用，而且測試上沒header來的方便。而 http 認證，是透過跳出的一認證的視窗的方式，也不是很適合在沒有browser下做操作，所以比較好的方式是採用 API key的方式來處理認證及保全(security)的問題。

API key是指，由server產生一個包含 username password跟相關資料的 token，然後在*每個* request的 parameter 或 header 中置入這個 token 讓 server 判斷 request 是否合法。

> token 放在 query string(request parameter)的方式
> 這種方式的優點是方便，直覺，容易測試，但缺點是每個url裡都會多出一堆資訊在url，而且會有安全跟cache的問題
> 建議 token 放header會比較好



要做權限管控，必須先了解需要管控的資源 (target resources) 有那些, 可要管控的等級，管控等級通常取決於登入時的角色，管控的等級大概如下:

1. Application Level : 是否存取資源, ex: login user 才能存取或 anonymous 就可存取 (authentication & authorization)
2. Module Level      : 是否以進入某個功能模組,底下可能會有許多子功能模組
3. Function Level    : 是否能使用對某個表單或單一功能, ex: 使用者管理
4. Instance Level    : 特定資料列的存取權限, ex: 系統中有10個使用者，但只有對其中 3 個有存取權限
5. Field Level       : 只針對特定欄位, ex: 使用者資料表中的敏感欄位(ex: 薪資) 只有特定人員可以存取

1 ~ 3 是屬於功能性的權限管控

4 ~ 5 是屬於資料的權限管控


選定了要採用要在每個 header 置入 token 的方式後，再來就是設計要放入什麼樣的 token 在 header 當作辨識的依據()，以及採用什麼方式保護及驗証 toekn 的安全性及有效性。

keyed-HMAC



Resources
---------
- <http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html>
- <http://devcenter.kinvey.com/rest/guides/security>
- <http://www.infoq.com/cn/news/2010/01/rest-api-authentication-schemes>
- <http://www.baeldung.com/security-spring>
#### deprecate

- <http://appsandsecurity.blogspot.tw/2011/04/rest-and-stateless-session-ids.html>
