---
author: Kent Chiu
published: true
layout: post
title: "Regular Expression (RE)"
date: 2012-09-12
comments: true
external-url:
sharing: true
footer: true
categories:
  - programming
  - re
---




### The Negated Character \^

Remember, a negated character class means “match a character that's not
listed” and not “don't match what is listed.”

Thus, [\^x] doesn't mean **“match unless there is an x”, but rather
match if there is something that is not x .** The difference is subtle,
but important. The first concept matches a blank line, for example,
while ![\^x] does not.

A character class, even negated, still requires a character to match.

### The Dot Charactor .

It can be convenient when you want to have an **any character here**
placeholder in your expression.

### Parenthesis ()

1.  capture
2.  grouping
3.  TBD

### Lookaround

-   lookahead left to right
-   lookbehind right to left

lookaround do **NOT** consume text

### Lookaround

\* (?=Expr) - zero-width positive lookahead 符合後面為exp的文字 \*
(?!Expr) - zero-width negative lookahead 符合後面沒接exp的文字 \*
(?⇐Expr) - zero-width positive lookbehind 符合前為為exp的文字
\*(?\<!Expr) - zero-width negative lookbehind 符合前面沒接exp字首的文字

#### Positive Lookaround

lookahead及
lookbehind所搜尋的是目前符合之前或之後的文字，並不包含目前符合本身。這些就如同”\^”及”\\b”特殊字元，本身並不會對應任何文字
(用來界定位置)，也因此稱做是zero-width assertions。

可以想像成SQL語法的**LIKE**或**NOT
LIKE**不過行為並儘相似，反正想到可用SQL
like來處理的問題，用Lookaround來處理一定可行。


```
    \b\w+(?=ing\b) (字尾為ing的字，比如說filling所符合的就是fill)
```

(?⇐exp)是一個”zero-width positive lookbehind
assertion”。它指的就是符合字首為exp的文字，但不包含exp本身。


```
    (?<=\bre)\w+\b (字首為re的字，比如說repeated所符合的就是peated)
    (?<=\d)\d{3}\b (在字尾的三位數字，且之前接一位數字)
    (?<=\s)\w+(?=\s) (由空白字元分隔開的字母數字字串)
```

#### Negative Lookaround

但如果只是要驗証某字元不存在而不要對應這些字元進來呢?舉個例子來說，假設要搜尋一個字，它的字母裏有q但接下來的字母不是u，可以用下列的RE來做。

```
\b\w*q[^u]\w*\b (一個字，其字母裏有q但接下來的字母不是u)
```

這樣的RE會有一個問題，因為[\^u]要對應一個字元，所以若q是字的最後一個字母，[\^u]這樣的下法就會將空白字元對應下去，結果就有可能會符合二個字，比如說”Iraq
haha”這樣的文字。使用Negative Lookaround就能解決這樣的問題。

```
\b\w*q(?!u)\w*\b (一個字，其字母裏有q但接下來的字母不是u)
```

這是”zero-width negative lookahead assertion”。

```
\d{3}(?!\d) (三個位元的數字，其後不接一個位元數字)
```

同樣的，可以使用(?\<!exp)，”zero-width negative lookbehind
assertion”，來符合前面沒接exp字首的文字串。

```
(?<![a-z ])\w{7} (七個字母數字的字串，其前面沒接字母或空格)
(?<=<(\w+)>).*(?=<\/\1>) (HTML標籤間的文字)
```

這使用lookahead及lookbehind
assertion來取出HTML間的文字，不包括HTML標籤。

### MISC

“03[-./]19[-./]76” 與 “03[.-/]19[.-/]76”不同，因為”-“會變rang的意思

找gary (gery) 可用”gr[ea]y”，”grey|gray”, and even
“gr(a|e)y”，但不能用”g[a|e]ry” 因為[a|e]會被當作 a or | or e

“gr(a|e)y”跟”gra|ey”是後者找的是gra或者是ey

.-\^ 等在character裡面跟外面有不同的意義

可有可以的字用”?” 如 ”(July|Jul)” 可用 “July?”. “4th|4” 可用 4(th)?

”\<H[1-6]
\*\>”.如果\*前面多一個空格，表示要空n格，如果沒有空格表示n個”]”

### \\p{class}, \\P{class}

\\p{class} - POSIX or unicode character class \\P{class} - exculde POSIX
or unicode character class

if pattern only contains \\p(or \\P) without {class}, it means all(or
excluded all) class

Supported class codes

C

Other

Cc

Control

Cf

Format

Cn

Unassigned

Co

Private use

Cs

Surrogate

L

Letter

Ll

Lower case letter

Lm

Modifier letter

Lo

Other letter

Lt

Title case letter

Lu

Upper case letter

M

Mark

Mc

Spacing mark

Me

Enclosing mark

Mn

Non-spacing mark

N

Number

Nd

Decimal number

Nl

Letter number

No

Other number

P

Punctuation

Pc

Connector punctuation

Pd

Dash punctuation

Pe

Close punctuation

Pf

Final punctuation

Pi

Initial punctuation

Po

Other punctuation

Ps

Open punctuation

S

Symbol

Sc

Currency symbol

Sk

Modifier symbol

Sm

Mathematical symbol

So

Other symbol

Z

Separator

Zl

Line separator

Zp

Paragraph separator

Zs

Space separator

中日韓文可以用 \\p{InCJKUnifiedIdeographs}

### TBD

\\i - Match of capture group i

\\cC - control character

\\G - previous match's end

\\A - The beginning of the input

&&

(Expr) - mark Expr as capture group

(ismd-ismd) - turn flags on or off

(ismd-ismd:Expr) - turn flags on or off in Expr

(?:Expr) - non capture group (有performance問題時才會使用)

(?\>Expr) - non capture atomic group

Lazy quantifiers
----------------

1.  ??
2.  +?
3.  \*?
4.  {n}?
5.  {n,}?
6.  {n,m}?

minimal matching, non-greedy, and un-greedy quantifiers.

Possessive quantifiers
----------------------

1.  ?+
2.  ++
3.  \*+
4.  {n}+
5.  {n,}+
6.  {n,m}+

Only Java supports those quantifiers now

Regex Modes and Match Modes
---------------------------

1.  /i - case-insensitive mode
2.  /x - free-spacing and comments mode
3.  /s - dot matches-all match mode (a.k.a., single-line mode)
4.  /m - enhanced line-anchor match mode (a.k.a., multiline mode)
5.  \\Q…\\E - literal-text regex mode

#### free-spacing and comments mode

all spaces and anything after comment mark(\#) will not interpreted.

if you need a space in pattern, using \\s instead normal space
character.

#### dot matches-all match mode (a.k.a., single-line mode)

Usually, dot does not match a newline, but in this mode, dot does.

#### \\Q...\\E - literal-text regex mode

\\Q, \\E means start/end quoting.

the contents of which have all meta-characters ignored (expect the \\E
itself).

### mode modifier (?modifier)

(?modifier) could be (?i) or (?-i) to enabled/disabled case-sensitive
matching.

the modifier can be i, x ,m ,s mode of [Regex Modes and Match
Modes](#regex_modes_and_match_modes "prog:regular_expression ↵")

Capture (\\1) ,(?\<Name\>...) or(?P\<Name\>...)
-----------------------------------------------

Here's a **PHP** regular expression for matching \*nested\* parentheses
(e.g. blocks of code):

```
    ((?:[^()]++|\((?1)\))*)
```

The ?1 is a recursive reference to the regex marked by the outermost
parentheses. It is a feature of the PHP regex engine.

See Jeffrey Friedl's Mastering Regular Expressions, 3rd ed., p. 476,
“Recursive reference to a set of capturing parentheses”.

Another potentially useful regex technique is “named capture”:

```
    ^(?P<protocol>https?)://(?P<host>[^/:]+)(?::(?P<port>\d+))?
```

Here you can use either \$matches[0], \$matches[1], \$matches[2] or
\$matches['protocol'], \$matches['host'], \$matches['port'].

This favour has difference implementation in difference language
(library), you should check manual of yours.

Greediness (貪婪)
-----------------

regexp 的搜尋引擎有一個特性, 叫做 greediness – 能吃多少,
就盡量吃多少。克服貪婪可用以下方式

```
[^...] "除了 ... 之外的任何一個字元"    
```

也可利用[lazy greedy](#lazy_quantifiers "prog:regular_expression ↵")

```
.??  或 .*? 
```

Lazy quantifiers是要用在`.*`後面，end of match的前面


```
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Insert title here</title>
    </head>
    <body>
    <!-- #BeginLibraryItem "/library/page_header.lbi" -->
    content of /page_header.lbi
    <!-- #EndLibraryItem -->
    <!-- #BeginLibraryItem "/library/page_center.lbi" -->
    content of /page_center.lbi
    <!-- #EndLibraryItem -->
    <!-- #BeginLibraryItem "/library/page_footer.lbi" -->
    content of /page_footer.lbi
    <!-- #EndLibraryItem -->
    </body>
    </html>
```

在使用Single-Line (Dot All) mode下，
如果是以上面這個RE做條件，會從第8行選到第16行，所以這樣會有三個matchs

```
<!--\s*#BeginLibraryItem.*#EndLibraryItem\s*-->
```

```
<!-- #BeginLibraryItem "/library/page_header.lbi" -->
content of /page_header.lbi
<!-- #EndLibraryItem -->
<!-- #BeginLibraryItem "/library/page_center.lbi" -->
content of /page_center.lbi
<!-- #EndLibraryItem -->
<!-- #BeginLibraryItem "/library/page_footer.lbi" -->
content of /page_footer.lbi
<!-- #EndLibraryItem -->
```

但是如果利用lazy quantifiers `*?`
，就只會選到第一個\#EndLibraryItem(第8行到第10行)，所以這樣會有三個matchs

```
<!--\s*#BeginLibraryItem.*?#EndLibraryItem\s*-->
```

```
<!-- #BeginLibraryItem "/library/page_header.lbi" -->
content of /page_header.lbi
<!-- #EndLibraryItem -->
```

```
<!-- #BeginLibraryItem "/library/page_center.lbi" -->
content of /page_center.lbi
<!-- #EndLibraryItem -->
```

```
<!-- #BeginLibraryItem "/library/page_footer.lbi" -->
content of /page_footer.lbi
<!-- #EndLibraryItem -->
```

The Backtracking
----------------

**TBD**

參考資源
--------

-   洪朝貴教授的[一輩子受用的 Regular
    Expressions](http://fsoss.fcu.org.tw/2004/hong-chaogui/08-regexp.html "http://fsoss.fcu.org.tw/2004/hong-chaogui/08-regexp.html")
-   [PHP patter
    modifierss](http://tw2.php.net/manual/en/reference.pcre.pattern.modifiers.php "http://tw2.php.net/manual/en/reference.pcre.pattern.modifiers.php")
-   [PHP RE
    details](http://tw2.php.net/manual/en/regexp.reference.php "http://tw2.php.net/manual/en/regexp.reference.php")
-   [http://www.regular-expressions.info/completelines.html](http://www.regular-expressions.info/completelines.html "http://www.regular-expressions.info/completelines.html")
    - a lot of RE examples

