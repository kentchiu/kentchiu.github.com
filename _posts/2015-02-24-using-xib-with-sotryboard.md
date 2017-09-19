---
published: true
author: Kent Chiu
layout: post
title: "在storyboard裡使用xib的view"
date: 2015-02-24 18:20
comments: true
sharing: true
footer: true
tags: 
- apple
- swift
- storyboard
- xib
- xcode
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

### 使用方式

1. 建立 XIB
2. 建立 swift class 繼承自 UIView ()
3. 設定 XIB 的 File's Onwer 為上述的 class (特別注意: XIB上的view的class 需是UIView 而不是 subclass view(MyView in this class),否則啟動會出錯


----------------------------------------------------------------

custom view 套用這個範本後即可在 storyboard 裡面 live rendering.

'''swift
@IBDesignable class MyView: UIView {

   
    var view: UIView!
    
    override init(frame: CGRect) {
        // 1. setup any properties here
        
        // 2. call super.init(frame:)
        super.init(frame: frame)
        
        // 3. Setup view from .xib file
        xibSetup()
    }
    
    required init?(coder aDecoder: NSCoder) {
        // 1. setup any properties here
        
        // 2. call super.init(coder:)
        super.init(coder: aDecoder)
        
        // 3. Setup view from .xib file
        xibSetup()
    }
    
    func xibSetup() {
        view = loadViewFromNib()
        
        // use bounds not frame or it'll be offset
        view.frame = bounds
        
        // Make the view stretch with containing view
        view.autoresizingMask = [.flexibleWidth, .flexibleHeight]
        
        // Adding custom subview on top of our view (over any custom drawing > see note below)
        addSubview(view)
    }
    
    func loadViewFromNib() -> UIView {
        let bundle = Bundle(for: type(of: self))
        // load MyView.xib
        let nib = UINib(nibName: "MyView", bundle: bundle)
        guard let view = nib.instantiate(withOwner: self, options: nil)[0] as?  UIView else { fatalError("cast to UIView Fail") }
        return view
    }
    
    /*
     // Only override drawRect: if you perform custom drawing.
     // An empty implementation adversely affects performance during animation.
     override func drawRect(rect: CGRect) {
     
     // If you add custom drawing, it'll be behind any view loaded from XIB
     }
     */

}   
'''

### 將上面的元件應用到storyboard上

拖拉一個 uiview，把class設成上面的cass即可



site: <http://iphonedev.tv/blog/2014/12/15/create-an-ibdesignable-uiview-subclass-with-code-from-an-xib-file-in-xcode-6>

# Resource
- <http://iphonedev.tv/blog/2014/12/15/create-an-ibdesignable-uiview-subclass-with-code-from-an-xib-file-in-xcode-6> - 人家寫好的教學, 含影片
- 另一個教學，影片版，但沒有採用 Live Rendering 的功能
  1. <https://www.youtube.com/watch?v=o3MawJVxTgk> - part1
  2. <https://www.youtube.com/watch?v=2ESU5hVRBPc> - part2
  3. <https://www.youtube.com/watch?v=pDHPA-CnuRw> - part3
