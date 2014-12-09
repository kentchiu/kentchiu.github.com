---
author: Kent Chiu
published: true
layout: post
title: "How to naming?"
date: 2011-10-17
comments: true
external-url:
sharing: true
footer: true
categories:
  - programming
---


# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------



Role-Suggesting Name pattern:

the names should be optimized for readability, not ease iof typing.

A good name should tell you

1.  Why it exists.
2.  Wait it does.
3.  How it used.

How to nameing:

1.  What is its purpose in the computation?
2.  How is the object referred to by the variable used?
3.  What is the scope and lifetime of the variable?
4.  How widely is the variable referenced?

If a name requires a comment, then the name does not reveal its intent.
If one has to struggle to find a name, it is generally because who don't
understand the computation very well. If the names is too long, it
reflects there are some design problem. If a method easy be naming, it
can fit to specification more exactly.

-   The word variable should never appear in a variable name.
-   The word table should never appear in a table name.

-   Use Intention-Revealing Names
-   Use Pronounceable Names
-   Use Searchable Names

Some useful same:

-   result, results
-   each
-   count

#### Composed Method pattern

Get your code working, then decide how it should be structured. If got
problem in composed method, then inline all and re-composed again.

#### Intention-Revealing Name pattern

Methods should be named for the purpose rather the implemention. There
is valuable to review the method is fit to the story which you want to
tell after refactoring.

Think about methods' names based on how they look in calling code. Bad
practice:


```
Customer.linearcustomerSearch(String id)

```

Godd practice:


```
Customer.find(String id)

```

The length of a name should correspond to the size of its scope

Why not Hungarian Notation? 1. Historical reason. (Window C API) 2. too
many type in modern languate (ex:Java.) 3. Strong type. 4. IDE.

Use Factory instead of complex constructor. (and make constoructor
private)

#### Library Class pattern

Library is :the place where programmer put the common codes


```
People usually uses static method to implement Library Class (utility class), but to use object is a better way.

```


```
the static method way:
public static void method(...params...) {
    ...some logic...
}

```


```
the object way:
public static void method(...params...) {
    new Library().instanceMethod(...params...);
}
private void instanceMethod() {
... some logic...
}

```

#### Common State pattern


```
Sometime only few subset of class use a variable, it no need to declare as field varible. it can be declare as paramter or Helper Object.

```

#### Local Variable pattern


```
The role of local variable:
1. Collector
2. Count
3. Explaining
4. Reuse

```

if a vaiable used for explaining, it maybe extract to a method.

#### Collecting Parameter pattern

Node asList() {


```
List results = new ArrayList();
addTo(results);
return results;

```

} addTo(List elements) {


```
elements.add(getValue());
for(Node each: getchildren())
    each.addTo(elements);

```

}

#### Explaining Message pattern

Sometimes the helper methods invoked by explaining message become
valuable points for further exetension. ex: highlight(Rectangle area) {
*→ it is make sense for user who wants to invoke this API. but no clue
to show how this it implemented. reverse(area);* → it is so clear how
this method work. }

#### Guard Clause pattern

Using Guard Clause to improve readability.

if (isInitilized()) {


```
return;

```

}

#### Checked Exceptions Pattern

the con of Check Exception: 1. These can easily add 50% to the length of
method. 2. Checked Exceptions also make changing code more difficult. 3.
Refactoring code with ckecked exception is more difficult.

#### Method Return Type pattern

Pick the most abstract return type that expresses your intention.

#### Method Comment pattern

Why Method Comment is better then code comment: 1. Updateability 2.
feedback

Complete Constructor.

​1. new Rectangle(0,0,50,200)

​2. Rectangle box = new Rectangle(); box.setLeft(0); box.setWidth(50);
box.setHeight(200); box.setTop(0)

the method 1 is more expression then 2. If method 1 is too complicate,
introduce the factory method instead.

#### Query Method pattern

When an object needs to offer decision criteria as part of its protocal,
do so with a method whose name is prefixd with form of “be”(like
“is”,”was” or “have”);

#### Setting Method pattern

​1. paragraph.setJustification(Paragraph.CENTERED) 2.
paragraph.centered();

Paragraph:centerd() {


```
setJustification(CENTERED);

```

}

1 is better then 2.

Resource
-----------
1. <http://www.javacodegeeks.com/2014/09/typical-mistakes-in-java-code.html>
