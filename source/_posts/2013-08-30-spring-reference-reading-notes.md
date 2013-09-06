---
published: false
author: Kent Chiu
layout: post
title: "Spring Reference Reading Notes"
date: 2013-08-30 11:37
comments: true
sharing: true
footer: true
categories: 
---


IoC container
-------



Spring 建議使用Setter Injection，Setter Injection有以下的優點
-	不會造成一堆constructor或長長的constructor parameters
-	可以避免 Circular Dependencies的引發exception問題；Bean A 依賴 Bean B, Bean B又依賴 Bean A(雞生蛋，蛋生雞問題)，這在 constructor 跟 field injection 會引發 exception，但在 setter injection不會
-	使用setter 或 constructor inject的方式會比較容易測試，因為不需要其他service locator (IOC container本身就是一種 service locator)



設定檔的路徑，應該要用絕對路徑，而非相對路徑

> spring 有些機制是透過參數名稱來做對應，這個是利用 complie 時有加入 debug flag，如果debug flag關掉了，這個功能就無效了
> ex: 
> 		
> 		<bean id="exampleBean" class="examples.ExampleBean">
>			<constructor-arg name="years" value="7500000"/>
>			<constructor-arg name="ultimateanswer" value="42"/>
>		</bean>
>
> `years`跟`ultimateanswer` 兩個是java class constructor的參數名稱，spring可以透過 complied 後的 debug info正確的把值派給對應的參數



#### lifecycle ####

bean的初始化跟解構有三種方式

1. 	InitializingBean,DisposableBean
	傳統的方式，實作這兩個介面spring就會調用 callback method
2. 	@PostConstruct and @PreDestroy
	JSR250 的做法，可以跟spring解耦，建議採用這種方式
3. 	自行定義init method (ex: start(), stop())
	常用於整合即有的code，又不能改source的情況下，可用此方式
	
上面這三種方式，也可以混在一起用，混在一起用時，調用的順序如下

- 初始化 : @PostConstruct -> InitializingBean.afterPropertiesSet() -> custom configured init() 
- 解構: PreDestroy -> DisposableBean.destroy() -> custom configured destroy()

如果要知道 container 的啟動，停止，refresh 發生的時機，可以實作以下介面
 	
 	// container 啟動，停止
	public interface Lifecycle {
	
	  void start();
	
	  void stop();
	
	  boolean isRunning();
	
	}	

	// 重整，關閉
	public interface LifecycleProcessor extends Lifecycle {
	
	  void onRefresh();
	
	  void onClose();
	
	}
	
	// phase用來決定啟動或停止順序的值，愈小的愈先啟動，愈晚停止
	// 可以用來決定那個 object 要在那個 object 之後啟動，停止
	public interface Phased {
	
	  int getPhase();
	
	}
	
	
	public interface SmartLifecycle extends Lifecycle, Phased {
	
	  boolean isAutoStartup();
	
	  void stop(Runnable callback);
	
	}


此外，spring還提供一堆感知(xxx-Aware)的call back method，用來感知一些context的事件


- ApplicationContextAware         
- ApplicationEventPublisherAware
- BeanClassLoaderAware
- BeanFactoryAware
- BeanNameAware
- BootstrapContextAware
- EmbeddedValueResolverAware
- EnvironmentAware
- LoadTimeWeaverAware
- MessageSourceAware
- NotificationPublisherAware
- PortletConfigAware
- PortletContextAware
- ResourceLoaderAware
- ServletConfigAware
- ServletContextAware

看 class name 大概就可以見文思義，知道有那些合用的call back method可以用。基本原則就是，如果需要知道 spring 在什麼時間做了什麼事的需求，就可以在`org.springframework.context` 這個 package 找找看有沒有合用的 class




ORM
---

#### exception translation ####

exception translation 是將各個 ORM Framework 的 exception 轉換成 Spring 的 DataAccessException ，這樣做的好處是可以在各種不同的 ORM Framework ，各種不同的Database中使用相同的Exception。

宣告成 `@Repository` 就會自動套用 exception translation

	@Repository
	public class ProductDaoImpl implements ProductDao {
	
	    // class body here...
	
	}



有三種方式可以設定EntityManagerFacotry, EntityManagerFacotry是用來在程式裡取得EitityManager。

1. LocalEntityManagerFactoryBean 用在簡單的佈署環境(獨立的應該程式)或整合測試，使用上有不少限制，請[參閱文件](http://static.springsource.org/spring/docs/current/javadoc-api/)
2. JNDI - JAVA EE 環境下使用
3. LocalContainerEntityManagerFactoryBean 使用這個就有完成的JPA功能，可用在像tomcat，或整合測試環境，但在JAVA EE環境下可能會有一些衝突(JAVA EE會自動找META-INFO下的persistence.xml，但採用LocalContainerEntityManagerFactoryBean可以完全不用使用persistence.xml)

> load time weaver
> spring使用load time weaving在load time時用aop的技術來改善entity的結構，像把entity 1對多關係改成lazy loading
> 但有的orm framework (ex: hibernate) 本身就有這個機制了


#### EntityManger ####

> EntityManger V.S. EntityManagerFactory
> 在使用時要inject EntityManagerFactory or EntityManager? EntityManagerFactory 是thread-safe而EntityManager不是，但如果在spring使用，會保證每個EntityManager的instance是新的(shared EntityManager Proxy)，所以直接inject EntityManager，code會比較簡單(如果使用PersistenceContextType.EXTENDED，那entity manage就不是 thread-safe)
> 如果inject的是EntityManagerFactory，要用`@PersistenceUnit`而如果是EntityManager，就用`@PersistenceContext`

#### JpaDialect ####
使用JpaDialect可以enable一些vendor-specific的進階功能，預設的DefaultJpaDialect沒有提供特別的功能

