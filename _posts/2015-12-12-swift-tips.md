---
published: true
author: Kent Chiu
layout: post
title: "Swift Tips"
date: 2015-12-12 15:00
tags: 
- swift
---

## Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


----------------------------------------------------------------



# IOS 9 http/https 問題

執行時在console看到這樣的訊息

```
App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.
```

在info.plist加入`NSAppTransportSecurity`




```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    ...(略)
    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads</key>
        <true/>
    </dict>
    ...
</dict>
</plist>

```

# swift default value

大部份的語法都是只允許有 default value 的 parameter 要放在沒有 default value 的後面，
但 swift 允許有 default value 的 parameters 可以放前面，會這麼設計的原因，或許是最後面的 parameter 要留著做 trail closure 


```swift
    func add(x: Int = 100,  y: Int) -> Int {
        return x + y
    }
    
    func testDefaultValue() {
        XCTAssertEqual(add(1,y: 2), 3)
        // x = 100 (default value)
        XCTAssertEqual(add(y:3), 103)
    }
```



# naming rule

- 常用縮寫字全部大寫(URL)或全部小寫(url)，不要混用
- 能用類型推斷就用，避免冗餘資訊
- `self` 只在 settor 跟 non-escaping clousure 中使用
- 使用 init() 來做型別轉換
- singleton 是標準用法

    sharedInstance
    ```swift
    class ControversyManager {
        static let sharedInstance = ControversyManager()
    }
    ```

# 善用 extension 來組織程式

相對獨立的部份就可以考慮 extract 成 extension ,ex: 

- 用 `// MARK ` 標示的區塊 
- 同一個 `protocal` 內的 method



# class vs struct
struct 是 value type，class 是 reference type，所以 `let` 跟 `var` 對這兩種類型會有不一樣的效果

struct

- 用 `let`， struct會變成 constant，什麼都不能改
- 用 `var`， stract會變成 variable，可以改內容

class

- 用 `let`， class會變成 final的效果，不能再指派成其他的reference，但class的屬性是可以異動的
- 用 `var`， class 可以再指派成另一個refenrece ex: var car = Car("car 1"),  car = Car("car 2")，car的reference從 car 1 變成 car 2

mutating function : `mutating` 只能用於 `var struct`
```swfit 
struct Car {
    mutating func run() {
        print("running.....")
    }
}
let car = Car()
car.run() // complie-time error

var car2 = Car()
car2.run() // running....
```




# if let where 

用 where 取代 and

```swift
        let task = NSURLSession.sharedSession().dataTaskWithURL(NSURL(string: url)!) { (data, response, error) -> Void in
            if let httpResponse = response as? NSHTTPURLResponse where error == nil && data != nil {
                
            }
        }

```


# Self 的使用技巧

```swift
protocol Unique {
    var id: String! { get set }
}

extension Unique where Self: NSObject {
    
    init(id: String?) {
        self.init()
        
        if let identifier = id {
            self.id = identifier
        } else {
            self.id  = NSUUID().UUIDString
        }
    }
}

```
如果這邊不使用`Self: NSObject`, 就無法使用 `self.init()` 那就必須在 protocol 裡面多宣告`init()` method，這樣所有的 adapters 也必須跟著實作 `init()`





