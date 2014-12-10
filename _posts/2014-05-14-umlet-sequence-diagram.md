---
published: true
author: Kent Chiu
layout: post
title: "UMLet Sequence Diagram"
date: 2014-03-14 12:05
comments: true
sharing: true
footer: true
tags: 
- uml
- umlet
- sequence_diagram 
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------




![img_001][img_001]

	title: sample
	_alpha:A~id1_|_beta:B~id2_|_gamma:G~id3_
	id1->>id2:id1,id2
	id2-/>id1:async Msg.
	id3->>>id1:id1,id3
	id1.>id3:id1,id3:async return Msg
	id1->id1:id1:self
	// this comment will not be render
	iframe{:interaction frame
	id2->id3:id1,id3:async Msg.
	iframe}



`title` : 圖左上方的標題

## Life Cycle
`_alpha:A~id1_` : alpha 是instance name, A: 是class name, id1 是識別用的id，用來當作其他圖示的reference，加上 `_` 則會出現下畫線


## Message 

訊息流向的基本格式為: `line:active:name` ，可以分成三段

1. 第一段是訊息的線條
   訊息的線條又可分為

   - `->>` 同步，空心箭頭
   - `-/>` 非同步，單邊箭頭
   - '->>>' 同步，實心箭頭
   - '.>' 非同步，虛線
   - '->' 同步

2. 第二段是訊息是否活動中
   如果是活動中的訊息，會出現 execution specification (或者是 activation) 

3. 第三段是訊息的名稱，可省略

ex:

`id1->>id2:id1,id2` :  `->>` 表示同步訊息，從id1到id2
`id2-/>id1:async Msg` : `-/>` 表示非同步訊息，`async Msg`是訊息名稱
`3->>>1:1,3 ` : `->>>` 表示


## Interaction Fragment 
在 iframe 包來的範圍是 interaction fragment 


[img_001]: http://blog.kent-chiu.com/images/2014-05-14/umlet-sequence-diagram_001.jpg


## Resource
- <http://www.uml-diagrams.org/sequence-diagrams.html> uml圖示說明
 


