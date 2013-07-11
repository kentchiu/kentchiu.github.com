---
author: Kent Chiu
published: true
layout: post
title: "Google Guava - Collection 101"
date: 2012-03-20
comments: true
external-url:
sharing: true
footer: true
categories:
  - guava
  - java
---



Guava是google的一個開源項目，目的在提供常用工具類的API，字串處理，caching，primitives型別處理，I/O，併行(concurrnecy)處理，
還有超級好用的Collections處理。

本篇主要著重於Collection相關的使用介紹，有[另外一篇](http://wiki.kent-chiu.com/doku.php?id=java:google_guava_101 "java:google_guava_101")專門在介紹Collection之外其它的功能的用法。

Iterable & Iterators
====================

如果之前method的return值習慣用Collection, List, Set, Map,
建議可以改傳回Iteratable\<T\>

```
  // 原本是傳回集合
  public List<String> getData(){...};
  // 可以改成傳回Iterable (RECOMMAND)
  public Iterable<String> getData(){...};
  // 或改成傳回Iterator
  public Iterator<String> getData(){...};
```

雖然**大部份的API都是傳回Iterator(since JDK1.4)**, 在Hamcrest
lib中只有提供Iterable(since JDK1.5) base的lib，沒有Iterator base的
`IsIterableWithSize<E> extends FeatureMatcher<Iterable<E>, Integer>`
用Iterable應該會相對方便些，而且Iterable可以取出Iterator，而Iterator沒有辦法直接取得Iterable，而且實作Iterable可以直接餵給foreach

```
assertThat(data, Matchers.<MyClass>iterableWithSize(1));
```

#### Iterable v.s Iterator

是java
1.5引入，跟foreach搭配使用的`java.lang.Iterable<T>`，`java.util.Iterator<E>`則是java
1.2引入後，1.5加入泛型的能力。

Iterable是指含有Iterator的物件，而Iterator則是指向集合裡單一的instance。

如果對一個集合取兩次的Iterator，兩次的Iterator是各自獨立的。


```
    package java.util;
     
    /**
     * @since 1.2
     */
    public interface Iterator<E> {
        boolean hasNext();
     
        E next();
     
        void remove();
    }
```


```
    /** Implementing this interface allows an object to be the target of
     *  the "foreach" statement.
     * @since 1.5
     */
    public interface Iterable<T> {
     
        Iterator<T> iterator();
    }
```

#### Iterable Array ?

在java
5+，有新的[For-Each](http://docs.oracle.com/javase/1.5.0/docs/guide/language/foreach.html "http://docs.oracle.com/javase/1.5.0/docs/guide/language/foreach.html")
loop語法， 它可以用來iterate實作Iterable
interface的類別。通常是用來iterate Collection(List, Set,
Map,…)及array，但是
如果試著把array丟到其他Iterable類型的參數，卻行不通，why?

```
String myArray = new String[]{"foo", "bar", "baz"};
for(String each : myArray) { // this one works
 
}
 
doIterate(myArray); // this cause compiler level error
 
public doIterate(Iterable args) {
}
```

原因為array根本沒有實作Iterable，For-Each
Loop對array的支援是compiler的syntax
sugar，compiler會把for-each展開成一般的for-loop

如果需要讓Array讓成iterable，可以用Arrays.asList()

Function & Predicate
====================

Function跟Predicate是許多method的參數，像常用的應該是

#### Predicaten

Predicaten常用於

1.  過濾集合中*特定條件的元素*
    `Collections2.filter(Collection<E>, Predicate<? super E>)`
2.  搜尋集合中*特定條件的元素*
    `find(Iterator<T>, Predicate<? super T>, T)`
3.  定位集合中*特定條件的元素*
    `indexOf(Iterator<T>, Predicate<? super T>)`
4.  判斷集合中所有*特定條件的元素*都符合條件
    `Iterators.all(Iterator<T>, Predicate<? super T>)`
5.  判斷集合中是否包含任何*特定條件的元素*
    `any(Iterator<T>, Predicate<? super T>)`
6.  …

上述中的**特定條件的元素**就是用Predicate實作，Predicate是一個interface，宣告如下

```
public interface Predicate<T> {
  /**
   * Returns the result of applying this predicate to {@code input}. This method is <i>generally
   * expected</i>, but not absolutely required, to have the following properties:
   *
   * <ul>
   * <li>Its execution does not cause any observable side effects.
   * <li>The computation is <i>consistent with equals</i>; that is, {@link Objects#equal
   *     Objects.equal}{@code (a, b)} implies that {@code predicate.apply(a) ==
   *     predicate.apply(b))}.
   * </ul>
   *
   * @throws NullPointerException if {@code input} is null and this predicate does not accept null
   *     arguments
   */
  boolean apply(@Nullable T input);
 
  /**
   * Indicates whether another object is equal to this predicate.
   *
   * <p>Most implementations will have no reason to override the behavior of {@link Object#equals}.
   * However, an implementation may also choose to return {@code true} whenever {@code object} is a
   * {@link Predicate} that it considers <i>interchangeable</i> with this one. "Interchangeable"
   * <i>typically</i> means that {@code this.apply(t) == that.apply(t)} for all {@code t} of type
   * {@code T}). Note that a {@code false} result from this method does not imply that the
   * predicates are known <i>not</i> to be interchangeable.
   */
  @Override
  boolean equals(@Nullable Object object);
}
```

一般來說，就是在apply()裡實作需求就可，apply會依序將集合中的元件透過input傳入，apply一次會處理一個元素(input)，判斷input符不符合**特定條件的元素**，然後return
true或false

比如說，我們做一個判斷數值是否大於0的Predicate

```
    class IsGreateThenZero implements Predicate<Integer> {
        @Override
        public boolean apply(Integer input) {
            return input > 0;
        }
    }
```

這樣就可以用來

1.  過濾集合中*大於0的的元素*
    `Collections2.filter(myIntegerList, new IsGreateThenZero())`
2.  搜尋集合中*大於0的的元素*
    `find(myIntegerList,  new IsGreateThenZero())`
3.  定位集合中*大於0的的元素*
    `indexOf(myIntegerList, new IsGreateThenZero())`
4.  判斷集合是否全部*大於0的*
    `Iterators.all(myIntegerList, new IsGreateThenZero())`
5.  判斷集合中是否包含任何*大於0的元素*
    `any(myIntegerList, new IsGreateThenZero())`
6.  ….

另外兩個Predicate間可以做邏輯運算，像AND, OR, NOT …

比如說，我們再寫一個判斷數值是否小於10的Predicate

```
    class IsLessThenTen implements Predicate<Integer> {
        @Override
        public boolean apply(Integer input) {
            return input < 10;
        }
    }
```

這樣，我們也可以用來AND, OR來做出 `> 0 AND < 10`，`> 0 OR < 10`的新條件

```
IsGreateThenZeroAndLessThenTenPredicate = Predicates.and(new IsGreateThenZero(), new IsLessThenTen());
```

[Predicates](http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/base/Predicates.html "http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/base/Predicates.html")裡面還有不少做集合運算的Predicates可以使用。

#### Function

Function常用於transform(轉換)，把某個集合轉成另一個集合，比如說，把List\<Integer\>專成List\<String\>，或把List\<Integer\>轉成Set\<Integer\>，當然，實際上的用法會比這樣的例子複雜許多，
不過，基本上想要把一種集合的元素，轉成”另一種”集合的**另一種元素**，就可以用`Iterables.transform(Iterable<F>, Function<? super F, ? extends T>)`達到

Function的用法上跟[Predicate](#predicate "java:google_guava_-_collection_101 ↵")差不多，Function也是一個interface，內容如下

```
public interface Function<F, T> {
  /**
   * Returns the result of applying this function to {@code input}. This method is <i>generally
   * expected</i>, but not absolutely required, to have the following properties:
   *
   * <ul>
   * <li>Its execution does not cause any observable side effects.
   * <li>The computation is <i>consistent with equals</i>; that is, {@link Objects#equal
   *     Objects.equal}{@code (a, b)} implies that {@code Objects.equal(function.apply(a),
   *     function.apply(b))}.
   * </ul>
   *
   * @throws NullPointerException if {@code input} is null and this function does not accept null
   *     arguments
   */
  T apply(@Nullable F input);
 
  /**
   * Indicates whether another object is equal to this function.
   *
   * <p>Most implementations will have no reason to override the behavior of {@link Object#equals}.
   * However, an implementation may also choose to return {@code true} whenever {@code object} is a
   * {@link Function} that it considers <i>interchangeable</i> with this one. "Interchangeable"
   * <i>typically</i> means that {@code Objects.equal(this.apply(f), that.apply(f))} is true for all
   * {@code f} of type {@code F}. Note that a {@code false} result from this method does not imply
   * that the functions are known <i>not</i> to be interchangeable.
   */
  @Override
  boolean equals(@Nullable Object object);
}
```

Function的template有兩個參數F, T，代表著From, To,
跟Predicate一樣也是實作apply，不過，他的apply的return不是boolean，而是T(To)
所以，主要的實作就是做input型別F跟return 型別T之間的mapping

Collection
==========

[Collection
Javadoc](http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/package-summary.html "http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/package-summary.html")

-   Using Multimaps relpace Map\<Foo, Collection\<Bar» :
    key是唯一值,如果put不同的內容到value,value會一直累加而不是取代
-   MapMaker
-   BiMap : 雙向的map,可以由key取出value,也可以由value取出key
-   ForwardingObject : google
    collections大多宣告成final，無法改寫，而forwarding提供一個decorate的機制，
    可在既有的google collections上做延伸

Google Collection裡很多class都不能直接用new產生，必須透過create()
method來建立 ex: ” HashBiMap.create()”

The following codes is got from many places, you can check the
[Resources](#resources "java:google_guava_-_collection_101 ↵") section
for source and details.

### ImmutableXXX

```
ImmutableMap.<String, Object>builder().put("a", objectA)..put("b", objectB).build();
```

### Find in collection

```
find = Iterators.find(myCollection.iterator(), predicate);
find2 = Iterables.find(myCollection, predicate);
// find1 == find2
```

### Iterators & Iterables

[Iterators](http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/Iterators.html "http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/Iterators.html")跟[Iterables](http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/Iterables.html "http://guava-libraries.googlecode.com/svn/trunk/javadoc/com/google/common/collect/Iterables.html")
的功能差不多，都是在Collection進行Iterator的相關動作，ex:

##### 搜尋、過濾與定址取值

1.  filter
2.  find
3.  get
4.  getFirst
5.  getLast
6.  getOnlyElement
7.  indexOf
8.  skip
9.  partition
10. limit
11. paddedPartition
12. peekingIterator (不會造成Iterator.next())

##### 相加

1.  addAll
2.  concat

##### 條件測試

1.  all
2.  any
3.  contains
4.  elementsEqual
5.  contains
6.  isEmpty
7.  elementsEqual (集合內元素順序是否完全相等)

##### 移除

1.  removeAll
2.  removeIf
3.  retainAll

##### 其它

1.  consumingIterable
2.  cycle
3.  unmodifiableIterable
4.  frequency

##### 轉換

1.  **forArray**
2.  transform
3.  **forEnumeration**

### transform

將collection內容的類型轉換成另一種類型(ex: Double → String).
^[1)](#fn__1)^


```
    List<String> list1 = Lists.newArrayList("1", "2", "3");
    List<Double> list2 = Lists.transform(list1, new Function<String, Double>() {
       public Double apply(String from) {
          return Double.parseDouble(from);
       }
    });
    System.out.println(Joiner.on(" | ").join(list2));
     
    // result
    // 1.0 | 2.0 | 3.0
```

### Filter

利用**Predicate**對collection進行過濾


```
    List<String> list = Lists.newArrayList("A100", "B100", null, "B200");
    Iterable<String> filtered = Iterables.filter(list, new Predicate<String>() {
       public boolean apply(String input) {
          return input == null || input.startsWith("B");
       }
    });
     
    System.out.println(Joiner.on("; ").useForNull("B000").join(filtered));
     
    // result
    // B100; B000; B200
```

### Ording

利用**Compartor**進行排序,也提供`max(),min()`等功能

```
 Ordering.from(lastNameComparator);
```

### ComparatorOrdering

除了基本的比較功能外，還有**Ordering**的功能

### Predicate


```
    import static com.google.common.base.Predicates.and;
    import static com.google.common.base.Predicates.compose;
    import static com.google.common.base.Predicates.in;
    import static com.google.common.base.Predicates.not;
     
    List<String> list1 = Lists.newArrayList("1", "2", "3");
    List<String> list2 = Lists.newArrayList("1", "4", "5");
    List<String> list3 = Lists.newArrayList("1", "4", "6");
     
    boolean result = and( not( in(list1) ), in(list2), in(list3)).apply("1");
     
    System.out.println(result);  // false
     
    List<String> list1 = Lists.newArrayList("A1", "A2", "A3");
    boolean result = compose(in(list1), new Function<String, String>() {
       public String apply(String from) {
          return "A" + from;
       }
    }).apply("1");
     
    System.out.println(result);  // true
```

### Combining and Modifying Comparators


```
    public class Person {
       private String firstName;
       private String lastName;
     
       public Person(String firstName, String lastName) {
          this.setFirstName(firstName);
          this.setLastName(lastName);
       }
     
       @Override
       public String toString() {
          return getFirstName() + " " + getLastName();
       }
     
       public void setFirstName(String firstName) {
          this.firstName = firstName;
       }
     
       public String getFirstName() {
          return firstName;
       }
     
       public void setLastName(String lastName) {
          this.lastName = lastName;
       }
     
       public String getLastName() {
          return lastName;
       }
    }
```


```
    List<Person> persons = Lists.newArrayList(
       new Person("Alfred", "Hitchcock"),
       null,
       new Person("Homer", "Simpson"),
       new Person("Peter", "Fox"),
       new Person("Bart", "Simpson"));
     
    Comparator<Person> lastNameComparator = new Comparator<Person>() {
       public int compare(Person p1, Person p2) {
          return p1.getLastName().compareTo(p2.getLastName());
       }
    };
     
    Comparator<Person> firstNameComparator = new Comparator<Person>() {
       public int compare(Person p1, Person p2) {
          return p1.getFirstName().compareTo(p2.getFirstName());
       }
    };
     
    // order by last name ascending
    Ordering<Person> ordering = Ordering.from(lastNameComparator);
    System.out.println(ordering.nullsLast().sortedCopy(persons));
     
    // order by last name descending, first name ascending
    ordering = ordering.reverse().compound(firstNameComparator);
    System.out.println(ordering.nullsLast().sortedCopy(persons));
```

### Constraints

TBD

### ForwardingObject

![guava_101_001.png][guava_101_001.png]

ForwardingObject是一種[Decorator\_pattern](http://en.wikipedia.org/wiki/Decorator_pattern "http://en.wikipedia.org/wiki/Decorator_pattern")的實作，而在guava裡，extends自ForwardingObject的類別都是collection，也就是說
在guava裡ForwardingObject是設計來改寫collection的預設行為的。

example: 改變原來LIST的的add()
method，如果加入null的話，就採用預設值”UNKNOWN”

```
import java.util.*;
import com.google.common.collect.*;
 
public class ListWithDefault<E> extends ForwardingList<E> {
    final E defaultValue;
    final List<E> delegate;
 
    ListWithDefault(List<E> delegate, E defaultValue) {
        this.delegate = delegate;
        this.defaultValue = defaultValue;
    }
    @Override protected List delegate() {
        return delegate;
    }
    @Override public E get(int index) {
        E v = super.get(index);
        return (v == null ? defaultValue : v);
    }
    @Override public Iterator<E> iterator() {
        final Iterator<E> iter = super.iterator();
        return new ForwardingIterator<E>() {
            @Override protected Iterator<E> delegate() {
                return iter;
            }
            @Override public E next() {
                E v = super.next();
                return (v == null ? defaultValue : v); 
            }
        };
    }
}
 
public static void main(String[] args) {
    List<String> names = new ListWithDefault<String>(
        Arrays.asList("Alice", null, "Bob", "Carol", null),
        "UNKNOWN"
    );
 
    for (String name : names) {
        System.out.println(name);
    }
    // Alice
    // UNKNOWN
    // Bob
    // Carol
    // UNKNOWN
 
    System.out.println(names);
    // [Alice, null, Bob, Carol, null]
}
```

Decorator
Pattern的精神在不改變原來類別的行為的基礎下，為該類別加入其他功能。
比如說，原來的類別可以印出 “hello
word”，如果套入一個叫”StarDecorator”，那原來的輸出會變成 “\* hello word
\*” 如果再套上”BarDecorator”那輸出就會變成 “|\* hello word \*|”

也就是不改成原來印出 “hello word”的功能下，裝飾上星型邊框
，再裝飾上條狀邊框，這也就是這個pattern的名稱的由來，

Resources
=========

-   [Guava
    Home](http://code.google.com/p/guava-libraries/ "http://code.google.com/p/guava-libraries/")
-   [各功能的使用介紹](http://code.google.com/p/guava-libraries/wiki/ExplainedContents "http://code.google.com/p/guava-libraries/wiki/ExplainedContents")
-   [Examples](http://jnb.ociweb.com/jnb/jnbApr2010.html "http://jnb.ociweb.com/jnb/jnbApr2010.html")
-   [Google
    Collections](http://jnb.ociweb.com/jnb/jnbFeb2009.html "http://jnb.ociweb.com/jnb/jnbFeb2009.html")
-   [Google’s guava java: the easy
    parts](http://www.copperykeenclaws.com/googles-guava-java-the-easy-parts/ "http://www.copperykeenclaws.com/googles-guava-java-the-easy-parts/")
-   [guava on
    stackoverflow](http://stackoverflow.com/questions/tagged/guava "http://stackoverflow.com/questions/tagged/guava")
-   Using the Google Collections Library for Java
    [part1](http://www.youtube.com/watch?v=ZeO_J2OcHYM "http://www.youtube.com/watch?v=ZeO_J2OcHYM")
    [part2](http://www.youtube.com/watch?v=9ni_KEkHfto "http://www.youtube.com/watch?v=9ni_KEkHfto")
-   带你领略 Google Collections [part
    1](http://jubin2002.javaeye.com/blog/471661 "http://jubin2002.javaeye.com/blog/471661")
    [part
    2](http://jubin2002.javaeye.com/blog/471698 "http://jubin2002.javaeye.com/blog/471698")
-   MapMaker Usage
    [http://norther.javaeye.com/blog/670414](http://norther.javaeye.com/blog/670414 "http://norther.javaeye.com/blog/670414")
-   [Google Guava Collections
    使用介绍](http://www.ibm.com/developerworks/cn/java/j-lo-googlecollection/index.html?ca=drs- "http://www.ibm.com/developerworks/cn/java/j-lo-googlecollection/index.html?ca=drs-")
    - IBM 中國上介紹guava的文章
-   [一些guava使用上的介紹](http://www.tfnico.com/presentations/google-guava "http://www.tfnico.com/presentations/google-guava")




[guava_101_001.png]: http://blog.kent-chiu.com/images/2012-03-20/guava_101_001.png
