---
published: true
author: Kent Chiu
layout: post
title: "Layout Constraint (NSLayoutAnchor)"
date: 2015-09-22 10:30
tags: 
- ios
- layout
---

## Layout

用程式做 layout 有以下方式
1. NSLayoutAnchor (iOS 9) -> 如果沒有相容性問題，用這種方式
2. NSLayoutConstraint (iOS 6)
3. Visual Format Langue  (iOS 6)
4. third party freamework : [SnapKit](https://github.com/SnapKit/SnapKit)




### Layout Constraint (NSLayoutAnchor)

![](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/Art/view_formula_2x.png)

紅色view.前面 = 1.0 倍 * 藍色view.後面 + 8 個points

紅色的view的前面，緊接在藍色view的後面8個points的位置

反過來說也成立: 

藍色的view後面points的位置緊接著紅色的view

下面這個是設定讓 title label跟他的supper view一樣大，滿直覺的寫法跟用Interface Buider拉的效果一致性很高

```swfit
        let margin = self.layoutMarginsGuide
        
        titleLabel.topAnchor.constraintEqualToAnchor(margin.topAnchor).active = true
        titleLabel.bottomAnchor.constraintEqualToAnchor(margin.bottomAnchor).active = true
        titleLabel.leftAnchor.constraintEqualToAnchor(margin.leftAnchor).active = true
        titleLabel.rightAnchor.constraintEqualToAnchor(margin.rightAnchor).active=true
```

#### layout guide and layout margin

![](http://blog.kent-chiu.com/images/2015-09-22/layout-constraint-004.jpg)

#### content hugging and compression resistance (CHCR) priorities

- content hugging -> 內容不想被延展時用這個控制, priority越高越不會被延展
- content compression resistance -> 內容不想被縮小時用這個控制, priority越高越不會被壓縮

```
|---- supper view (priority = 500) ----|

# hugging priority > 500，button 變成緊實(compact)
[A Button]
# hugging  priority < 500，button 被延展到跟supper view一樣大
[                A Button              ]

# compression resistance priority > 500, 內容沒被壓縮
[A Button]
# compression resistance priority < 500, 內容被壓縮
[A But..]
```


```
# content hugging priorities
|-------------|-------------|
  250             250       
[Label]          [Text]     

   251            249       
[Label][Text               ]
   250            251       
[Label               ][Text]

# content compression resistance priorities
        750            750
[      Image      ][ Label ]  # 預設
        750            750
[      Image      ][LongLab]  # label內容過長被壓縮了
        750            751
[    Image    ][ LongLabel ]  # 要求放不下時優先壓縮 Image 大小，而不是Label

```

### Tips

- 儘可能的用stack view，然後有需時再用 constraint (stackview 的一堆屬性是無效的，ex: backgroundColor)
- 不要 add/remove constraint，改用 activate / deactivate
- **絕對不要** deactivate self.view.constraints
- table view 的 size 可以自動記算，但 constraint 要設好 : see also [Mysteries of Auto Layout Part1](https://developer.apple.com/videos/wwdc/2015/?id=218) 
- 加入 layout identity 對 debuging layout 問題很有幫助
- layout的效果不如預期的原因是 constraints 不足或是 priority 衝突
- call `UIView.hasAmbiguosLayout` to diagnosis

## Stack View


#### Aligment

![](http://blog.kent-chiu.com/images/2015-09-22/layout-constraint-001.png)

![](http://blog.kent-chiu.com/images/2015-09-22/layout-constraint-002.png)

#### Distribution

![](http://blog.kent-chiu.com/images/2015-09-22/layout-constraint-003.png)


### Tips

- 部份元件會自動推算大小(intrinsic size), 所以在設定大小可以不用設定大小的 constraint
- 儘量用相對與其他 view 的方式決定位置, 然後由元件的 intrinsic size 決定大小
- 橘色的"T-bar" 表示 constraints 設定把完整,無法計算出 view 的位置及大小,需要再加入其他的 constraint (missing constraints),讓"T-bar"直至成為藍色為止
- 設定多個 constraints 的方式是按住 constraint 後多選數個 views 加入 constraint
- 部份元件,在沒有加入任何 constraint 前, layout 不會跑掉是因為 auto layout constraint 
- 先處理 sub view 間的 constraints 關係,讓它 "群組化"後再設定與 super view 會比較容易
- 如果要設定subview小於super view的一定比例以下,可在subview 與 super view 間拉 *Equal Heights Constraint* 或 *Equal Widths Constraint*, 然後設定 *Relation* 為 *Less Than or Equal* (可能需先對調 *First Item*, *Second Item*)
- 先弄出可以適用於所有 devices 的通用 layout, 在針對特定的layout調整, 如果能不用到 size class 就不要用
- font 的設定是全局的, 一旦設定就會影響到 base layout，如果要為特定的layout 設定專用的font, 必須為該layout新增一特定的font
- constraint 跟其他的 UI 元件一樣,也是可以透過 `@IBOutelet` 用程式控制


### Resources

- <http://xuexuefeng.com/autolayout/> - 詳細的auto layout說明(簡體中文)
- <http://www.jianshu.com/p/a4b8e0c8e68d>
- <http://stackoverflow.com/questions/15850417/cocoa-autolayout-content-hugging-vs-content-compression-resistance-priority>
- WWDC 2015 影片: Mysteries of Auto Layout [Part1](https://developer.apple.com/videos/wwdc/2015/?id=218),[Part2](https://developer.apple.com/videos/wwdc/2015/?id=219)
- stack view : <http://www.devtalking.com/articles/uistackview/>


