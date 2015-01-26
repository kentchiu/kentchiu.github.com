---
published: false
author: Kent Chiu
layout: post
title: "設定讓unit test 可以使用Cocoapods的dependency"
date: 2015-01-23 02:50
comments: true
sharing: true
footer: true
tags: 
- ios
- cocoapods
- unittest
---

Podfile

```
platform :ios, '8.1'

link_with 'App', 'AppTests'
pod 'Alamofire-SwiftyJSON' , :git => "https://github.com/SwiftyJSON/Alamofire-SwiftyJSON", :branch => "master"
pod 'Alamofire', '~> 1.1'
pod 'SwiftyJSON', '~> 2.1'
```

目前(2015/01/23)`Alamofire-SwiftyJSON`還沒進到cocoapods的repository，但專案本身有cocoapods的spec檔，所以使用上格式不太一樣

link_with 'App', 'AppTests' 的部份是指 MyApp, MyAppTest 這兩個target都可以吃到cocoapods的dependency，MyApp是 product target(通常就是專案名稱)，MyAppTest是 unit test的target



執行`pod install`後，開啟workspec檔，如可以在product code跟unit test code用lib了


把下面的code加入unit test執行看看

```swift

import Alamofire
import SwiftyJSON

...

    func testJSONResponse() {
        let URL = "http://httpbin.org/get"
        let expectation = expectationWithDescription("\(URL)")
        
        Alamofire.request(.GET, URL, parameters: ["foo": "bar"])
            .responseJSON { (req, res, json, error) in
                expectation.fulfill()
                XCTAssertNotNil(req, "request should not be nil")
                XCTAssertNotNil(res, "response should not be nil")
                XCTAssertNil(error, "error should be nil")
                var json2 = JSON(json!)
                XCTAssertEqual(json2["args"], SwiftyJSON.JSON(["foo": "bar"] as NSDictionary), "args should be equal")
        }
        
        waitForExpectationsWithTimeout(10) { (error) in
            XCTAssertNil(error, "\(error)")
        }
    }
```

`Alamofire.request` 是非同步的, 所以要用`waitForExpectationsWithTimeout`進行等待,直到` expectation.fulfill()`被觸發
