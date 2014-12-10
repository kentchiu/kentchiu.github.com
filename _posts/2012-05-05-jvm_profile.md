---
author: Kent Chiu
published: true
layout: post
title: "JVM 記憶體調校"
date: 2012-05-05
comments: true
external-url:
sharing: true
footer: true
tags:
  - mat
  - jvm
  - java
---



#### JVM記憶體相關參數

- Xmx                        : 記憶體使用上限 
- Xms                        : 記憶體使用下限(啟始值) 
- Xss                        : stack 
- server                     :                        
- client                     :                        
- XX:PermSize                : Perm for Permanent:編譯後的class，或固定的資料結構會佔用這一個值 
- XX:MaxGCPauseMillis        : GC暫停的毫秒數，這個只是"建議"GC而已，GC會儘量滿足這個建議值 
- XX:+ScavengeBeforeFullGC   :                        
- XX:-UseParallelOldGC       :                        
- XX:NewRatio                : sets the size of the old generation to three times the size of the new generation 


![jvm_profile_01.jpg][jvm_profile_01.jpg]

圖片來源 :[Red Stack's Blog](http://redstack.wordpress.com/ )

GC就是在空間不足時，將不活躍的物件從記憶體中移除，如果在GC時存活下的物件，就可能很上晉昇(Young
→ Old → Permanent)

1.  Permanent Generation:Old Generation進行GC(major GC or full
    GC)後，如空間不夠，就會將Old
    Generation活躍的物件(未被GC的)移到這區。建立越多的物件，此區域就要越大
2.  Old Generation: 當Young Generation進行gc (minor
    GC)後，空間仍舊不夠放新建立的物件時，會將Young Generation移到Old
    Generation
3.  New(Young) Generation():
    1.  to: Survivor space1(S1)
        gc時,會將eden區未被gc的物件COPY到S1或S2區
    2.  from: Survivor space2(S2)
        gc時,會將eden區未被gc的物件COPY到S1或S2區
    3.  eden: (伊甸) 看名字就知道是新生命(New
        Object)誕生的地方,每new一個Object時會佔用這個區域，eden滿了，會引發gc
        (minor GC)

Heap= Old Generation + New Generation

可以透過JDK內附的virtualVm來觀察jvm memory的使用情形

![jvm_profile_02.png][jvm_profile_02.png]

### 常用工具

1.  jinfo
2.  jmap
3.  jstack
4.  jstat
5.  java.lang.management (API level)



                    

Resources
---

-   [簡單而清楚的JVM
    Memory教學](http://redstack.wordpress.com/2011/01/06/visualising-garbage-collection-in-the-jvm/ "http://redstack.wordpress.com/2011/01/06/visualising-garbage-collection-in-the-jvm/")
-   [infoQ中國站關於gc的文章(簡體中文)](http://www.infoq.com/cn/articles/cf-java-garbage-references "http://www.infoq.com/cn/articles/cf-java-garbage-references")
-   [http://java.sun.com/docs/hotspot/gc/](http://java.sun.com/docs/hotspot/gc/ "http://java.sun.com/docs/hotspot/gc/")
-   [10 jvm options java developers should
    know](http://javarevisited.blogspot.com/2011/11/hotspot-jvm-options-java-examples.html "http://javarevisited.blogspot.com/2011/11/hotspot-jvm-options-java-examples.html")
-   [Eclipse MAT](http://eclipse.org/mat/ "http://eclipse.org/mat/") -
    這個比virtualVM更好用
-   [Eclipse
    MAT使用的文章](http://kohlerm.blogspot.com/2009/07/eclipse-memory-analyzer-10-useful.html "http://kohlerm.blogspot.com/2009/07/eclipse-memory-analyzer-10-useful.html")
-   [GC](http://www.cubrid.org/blog/dev-platform/understanding-java-garbage-collection/ "http://www.cubrid.org/blog/dev-platform/understanding-java-garbage-collection/")
    - 說明GC的機制及時機，有5種gc策略的說明
-   [如何監控GC的教學](http://www.cubrid.org/blog/dev-platform/how-to-monitor-java-garbage-collection/ "http://www.cubrid.org/blog/dev-platform/how-to-monitor-java-garbage-collection/")





[jvm_profile_01.jpg]: http://blog.kent-chiu.com/images/2012-05-05/jvm_profile_01.jpg
[jvm_profile_02.png]: http://blog.kent-chiu.com/images/2012-05-05/jvm_profile_02.png

