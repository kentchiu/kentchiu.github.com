---
published: true
author: Kent Chiu
layout: post
title: "Java 8 lambda"
date: 2014-03-14 12:47
comments: true
sharing: true
footer: true
categories: 
- java8
- lambda
- clojure
- functional programming
---


基本語法:

``` java
        (parameters) -> expression
    or
        (parameters) -> { statements; }
 
    (parameters) -> expression
    (x, y) -> x + y;    // 計算 x + y 的結果
    (String name) -> System.out.println("Hi! " + name); 

    (parameters) -> { statements; }
    x -> { x + 1; x + 2; x + 3; return x; } 
```

參數的型別如果沒有指定的，會進行類型推斷(type infer)




#### functional interface
只有定義一個抽象方法的interface叫"functional interface"，functional interface主要的用途是做 lambda expression.

在對lambda的使用還不熟悉時，可以先用anonymous class來一步一步轉換成lambda

``` java
    // Stream裡filter method的宣告，filter method需要傳入一個Predicate的interface
    public interface Stream<T> extends BaseStream<T, Stream<T>> {
        Stream<T> filter(Predicate<? super T> predicate);
    }    
     
    // Predicate主要是透過test這個method來解決結果是true，或false 
    @FunctionalInterface
    public interface Predicate<T> {
        /**
         * Evaluates this predicate on the given argument.
         *
         * @param t the input argument
         * @return {@code true} if the input argument matches the predicate,
         * otherwise {@code false}
         */
        boolean test(T t);
    }
```

sample:

``` java
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);
    
        // 先用anonymous實作，這邊就可以看出來，如果Predicate有超過一個的abstract method，
        Predicate<Integer> predicate = new Predicate<Integer>() {
            @Override
            public boolean test(Integer i) {
                return i > 3;
            }
        };
    
        // 執行後，results會只包含list中大於3的數，所以為4,5這兩個數字    
        List<Integer> results = list.stream().filter(predicate).collect(Collectors.toList());
    
        // 傳成lambda時，因為Predicate是function interface，只有一個abstract method，
        // 所以，我們可以很清楚的知道，我們要實作的method是`public boolean test(Integer i)`
        // 傳入的參數為Integer, 而實作的logic為判斷參數是否大於3
        Predicate<Integer> predicate = (Integer i) -> {return i > 3;};
        
        // java8的compier類型推斷(type infer)能力變強了，可以compiler可以由程式的上下文(context)猜出正確的型別，
        // lambda expression可以再簡化如下
        Predicate<Integer> predicate = (i) -> {return i > 3;};
        
        // 再把冗餘的括號點去掉，lambda expression預設會return最後一行的值，所以，return也可以去掉    
        Predicate<Integer> predicate = i -> i > 3;
    
        // 在filter中套上predicate
        List<Integer> results = list.stream().filter(predicate).collect(Collectors.toList());

        // 對predicate執行 inline variable的refactory
        List<Integer> results = list.stream().filter(i -> i > 3).collect(Collectors.toList());
```

在Predicate中，還有幾個default method，主要是用來輔助Predicate的使用，例如我們想取出上面相反的結果(不是大於3的值)，
直覺的方式，就是判斷的邏輯 `i>3` 改寫成 `!(i > 3)`，而Predicate.negate()這個default method就是在做這件事

``` java
        Predicate<Integer> predicate = i -> i > 3;  // 先把lambda express從filter method中extract出來
        Predicate<Integer> negate = predicate.negate(); // 取反向的值 
        List<Integer> results = list.stream().filter(negate).collect(Collectors.toList()); // 得到的results為 1, 2, 3
```

可以看到，我們用`predicate.negate()`就不用直接改寫原來的邏輯，只要直接對predicate做negate的運算即可。

Predicate還有幾個default methods都是在做Predicate運算時常會用到的，功用跟[guava lib中的Predicats](http://docs.guava-libraries.googlecode.com/git/javadoc/com/google/common/base/Predicates.html)類型，
Predicate function interface 的完整程式如下

``` java
        @FunctionalInterface
        public interface Predicate<T> {
            boolean test(T t);
    
            default Predicate<T> and(Predicate<? super T> other) {
                Objects.requireNonNull(other);
                return (t) -> test(t) && other.test(t);
            }
    
            default Predicate<T> negate() {
                return (t) -> !test(t);
            }
    
            default Predicate<T> or(Predicate<? super T> other) {
                Objects.requireNonNull(other);
                return (t) -> test(t) || other.test(t);
            }
            static <T> Predicate<T> isEqual(Object targetRef) {
                return (null == targetRef)
                        ? Objects::isNull
                        : object -> targetRef.equals(object);
            }
        }
```



#### lambda expression
A lambda expression is an instance of a functional interface

#### lambda expression常見的參數為

* Predicate：接受一個參數，對參數做評估後返回一個boolean的值 
  ex: 過濾條件時，傳入過濾條件的參數，如果成立(true) 就進行過濾
* Function：接受一個參數並產出結果
  ex: 輸入字串，返回數字
* Supplier：不接受参數，並返回結果
  ex: 對stream中的元素計算後產生資料
* Consumer：接受一個參數，但不返回結果(返回void)
  ex: 經function運算後，影響輸入的結果(副作用)


上面的class都是零元或一元運算子，還有二元運算子，功能類似，差異之處是在都是接受兩個參數

* BiConsumer
* BiFunction
* BiPredicate




RESOURCE
======
- http://www.techempower.com/blog/2013/03/26/everything-about-java-8/
- http://winterbe.com/posts/2014/03/16/java-8-tutorial/
- http://docs.oracle.com/javase/tutorial/collections/streams/reduction.html - reduce  




