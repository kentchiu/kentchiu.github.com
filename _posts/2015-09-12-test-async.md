---
published: true
author: Kent Chiu
layout: post
title: "iOS 非同步測試"
date: 2015-09-12 10:30
tags: 
- ios
- testing
---

程式會在還沒收到 response 前就結束了，所以`print`完全沒有被執行

```swift
    func testAsynchronous_no_stop() {
        Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
            .responseJSON {  response in
                // 執行時，還未進入這段程式，測試就結束了
                print(response)
        }
    }
```

使用 expectation/ fulfill / waitForExpectationsWithTimeout 來等待網路回應完成，然後用`fulfill`來結束等待，如果沒有使用`fulfill`，
testacase要 test fail 或 timeout 時才會結束

```swift
    func testAsynchronous_wait_for_fulfill_or_timeout() {
        let expectation = expectationWithDescription("get data")
        
        Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
        .responseJSON { response in
                print(response)
                XCTAssertNil(response.result.error)
                XCTAssertEqual(response.response?.statusCode, 200)
                XCTAssertNotNil(response.result.value)
            // expectation 條件已經行滿足，可以提早結束等待，不用等到timeout
            expectation.fulfill()
        }
        
        // 執完後會hold在這邊，直到timeout(10.0秒) 或 所有的 expectation(本例中只有一個)都被 fulfill
        waitForExpectationsWithTimeout(10.0, handler: nil)
    }
```


這是屬於 product code的實作，應該移出測試，我們只關心回傳的結果是成功或失敗，以及json的資料是不是正確，

```swift
    Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
            .responseJSON { request, response, json in
                print(request)
                print(response)
                print(json)
                XCTAssertEqual(response?.statusCode, 200)
                XCTAssertNotNil(json)
                // expectation 條件已經行滿足，可以提早結束等待，不用等到timeout
                expectation.fulfill()
        }
```

資料的callback是發生在 closure 裡，所以需透過把整個 closure extract 出來

product code

```swift
 class MyApiClient  {
        
        func jsonData(onCompletion :   (AnyObject) -> () )  {
            
            Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
                .responseJSON { _, _, result in
                    if let  data = result.value  {
                        onCompletion(data)
                    }
            }
        }
        
    }
```
    
test code

```swift
    func testClient() {
        let expectation = expectationWithDescription("get data")
        
        let client = MyApiClient()
        client.jsonData() { data in
            XCTAssertNotNil(data["args"])
            expectation.fulfill()
        }
        
        // 執完後會hold在這邊，直到timeout(10.0秒) 或 所有的 expectation(本例中只有一個)都被 fulfill
        waitForExpectationsWithTimeout(10.0, handler: nil)
    }
```



