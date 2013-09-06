---
published: false
author: Kent Chiu
layout: post
title: "Python List Comprehensions"
date: 2013-08-31 12:08
comments: true
sharing: true
footer: true
categories: 
- python
---

- list comprehensions 中文維基上翻為*列表概括*
- python2 引入 list comprehensions，而pythons 3 時擴充到 dictionary, set 

python list comprehensions 的語法如下:

	                  | ① ｜      ②       ｜          ③              ｜
	 squared_ints = [ e**2 for e in a_list if type(e) == types.IntType ]
list comprehensions 是可以放在list裡的表達式，所以，整個expression是在 list的list literal (`"["  "]"`)裡面

1. 要做什麼樣的運算
2. for … in 很制式的寫法,裡面的變數名稱要跟前面的①裡面的一致
3. 過濾條件，成立時才做運算



		>>> a_list = [1, ‘4’, 9, ‘a’, 0, 4]
		# 如果a_list裡的元素(e)的型別為int時，就該元素的2次方(e**2)
		>>> squared_ints = [ e**2 for e in a_list if type(e) == types.IntType ]
		[ 1, 81, 0, 16 ]
 
       







Resources
---------
- [dive into python 3](http://www.diveinto.org/python3/comprehensions.html)
- [wikipedia](http://en.wikipedia.org/wiki/List_comprehension)
- [Python 3 Patterns, Recipes and Idioms](http://python-3-patterns-idioms-test.readthedocs.org/en/latest/Comprehensions.html) 

