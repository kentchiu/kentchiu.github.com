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

### TIP

- 儘可能的用stack view，然後有需時再用 constraint
- 不要 add/remove constraint，改用 activate / deactivate
- **絕對不要** deactivate self.view.constraints
- table view 的 size可以自動記算，但constraint要設好 : see also [Mysteries of Auto Layout Part1](https://developer.apple.com/videos/wwdc/2015/?id=218) 

- <http://www.jianshu.com/p/a4b8e0c8e68d>
- <http://stackoverflow.com/questions/15850417/cocoa-autolayout-content-hugging-vs-content-compression-resistance-priority>
- WWDC 2015 影片: Mysteries of Auto Layout [Part1](https://developer.apple.com/videos/wwdc/2015/?id=218),[Part2](https://developer.apple.com/videos/wwdc/2015/?id=219)



