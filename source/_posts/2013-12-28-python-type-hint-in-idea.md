---
published: true
author: Kent Chiu
layout: post
title: "Python type hint in IDEA"
date: 2013-12-28 12:09
comments: true
sharing: true
footer: true
categories: 
- IDEA
- python
---

在現代的IDE中，程式碼自動完成(code assist or code complete)幾乎是必備的基本功能，這個功能在靜態功能型別的語言中，IDE通常可以很完全的運作；但是在動態型別的語言中，就常常沒辦法推斷出正確的型別了。
因為有的IDE從另外從程式碼外的其它地方(通常是註解)加入協助IDE做類型推斷(type infer)的動作。

IDEA的pythond plugin(或 pycharm)也是使用註解的做type hint來協助IDEA做類型推斷。如果是使用python3開發，那麼還可以用[PEP-3107](http://www.python.org/dev/peps/pep-3107/)。
*PEP-3107*在語言的級別上加入了參數跟傳回值的型別，這樣IDE就有辦法做類型的推斷。

	# 未採用PEP-3107的method宣告方式
	def a_method(foo, bar) :
		return foobar;

	# 採用PEP-3107的method宣告方式
	def a_method(foo : TypeFoo, bar: TypeBar) -> TypeFooBar

TypeFoo是參數foo的型別，TypeBar是參數bar的型別，而TypeFooBar則是 return value foobar的型別


但是以下幾種狀況是*PEP-3107*無法處理的:

- locale variable，如果 locale variable不是某個method的傳回值，那就沒有型別
- field ，field也沒有型別
- third party的lib，third party的lib寫法可能不是採用*PEP-3107*方式，所以ide也無法提供code complete

計對這些狀況，可以用一開始提到的方式，套用特定的註解來協助IDE做類型推斷。

    r = praw.Reddit(user_agent='User-Agent: rbot/1.0 by draculacwg')
    ''':type: six.Subreddit ''' 
    subreddit = r.get_subreddit(subreddits)
    submissions = subreddit.get_new()

原來的`subreddit.get_new()`，原本沒有code complete，加入`''':type: six.Subreddit '''` 後，就會有code complete了


#### IDEA 建議的type hinting 語法 

- Foo # Class Foo visible in the current scope
- x.y.Bar # Class Bar from x.y module
- Foo | Bar # Foo or Bar
- (Foo, Bar) # Tuple of Foo and Bar
- list[Foo] # List of Foo elements
- dict[Foo, Bar] # Dict from Foo to Bar
- T # Generic type (T-Z are reserved for generics)
- T <= Foo # Generic type with upper bound Foo
- Foo[T] # Foo parameterized with T
- (Foo, Bar) -> Baz # Function of Foo and Bar that returns Baz
- list[dict[str, datetime]] # List of dicts from str to datetime (nested arguments)

Resource
--------
-	<http://www.jetbrains.com/pycharm/webhelp/type-hinting-in-pycharm.html> - pycharm 或 IDEA python plugin中 type hint的方式
-   <http://www.python.org/dev/peps/pep-0257/> - PEP 257  : Docstring Conventions
-	<http://www.python.org/dev/peps/pep-3107/> - PEP 3107 : python 3 中可用的類型註解

