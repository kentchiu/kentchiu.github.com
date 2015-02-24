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


----------------------------------------------------------------

custom view 套用這個範本後即可在 storyboard 裡面 live rendering.

'''swift
   override init(frame: CGRect) {
        // 1. setup any properties here
        
        // 2. call super.init(frame:)
        super.init(frame: frame)
        
        // 3. Setup view from .xib file
        xibSetup()
    }
    
    required init(coder aDecoder: NSCoder) {
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
        view.autoresizingMask = UIViewAutoresizing.FlexibleWidth | UIViewAutoresizing.FlexibleHeight
        
        // Adding custom subview on top of our view (over any custom drawing > see note below)
        addSubview(view)
    }
    
    func loadViewFromNib() -> UIView {
        let bundle = NSBundle(forClass: self.dynamicType)
        let nib = UINib(nibName: "ChangeToYouViewName", bundle: bundle)
        
        // Assumes UIView is top level and only object in CustomView.xib file
        let view = nib.instantiateWithOwner(self, options: nil)[0] as UIView
        return view
    }

    /*
    // Only override drawRect: if you perform custom drawing.
    // An empty implementation adversely affects performance during animation.
    override func drawRect(rect: CGRect) {
    
    // If you add custom drawing, it'll be behind any view loaded from XIB
    
    
    }
    */
    
'''
site: <http://iphonedev.tv/blog/2014/12/15/create-an-ibdesignable-uiview-subclass-with-code-from-an-xib-file-in-xcode-6>

# Resource
- <http://iphonedev.tv/blog/2014/12/15/create-an-ibdesignable-uiview-subclass-with-code-from-an-xib-file-in-xcode-6> - 人家寫好的教學, 含影片
- 另一個教學，影片版，但沒有採用 Live Rendering 的功能
  1. <https://www.youtube.com/watch?v=o3MawJVxTgk> - part1
  2. <https://www.youtube.com/watch?v=2ESU5hVRBPc> - part2
  3. <https://www.youtube.com/watch?v=pDHPA-CnuRw> - part3
