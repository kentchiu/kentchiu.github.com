---
published: true
author: Kent Chiu
layout: post
title: "Spring Reference Reading Notes"
date: 2013-08-30 11:37
comments: true
sharing: true
footer: true
categories: 
---

# Table of contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

----------------------------------------------------------------


# IoC container

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


#### Container Extension Points ####

##### BeanPostProcessor #####

如果需要對特些特殊類似的 bean做處理，可以利用`BeanPostProcessor`做bean建立前，或建立後的處理,比如說，
我們可以所有的`@Schedule`的bean加上做排程。

	public interface BeanPostProcessor {
	
		Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
	
		Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;
	
	}

##### BeanFactoryPostProcessor #####

`BeanPostProcessor`做bean建立前，或建立後的處理，而`BeanFactoryPostProcessor`則是做bean definition(meta data)做修改，例如`PropertyPlaceholderConfigurer`就是把bean的定義裡的placeholder換成真正的屬性值

	public interface BeanFactoryPostProcessor {
	
		void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException;
	
	}

##### FactoryBean #####

如果建立bean的過程比較複雜，可以使用`FactoryBean`來建立bean


#### Annontation ####
- @Required   舊的，不要用了`@Autowired`裡required屬性具有相同的效果
- @Autowired  使用後會依型別自動做DI
- @Qualifier  spring會依型別做auto wried，但常會遇過一個interface，有一個以上的實作，或者有子類別，子子類別，這時，可以用`@Qualifier`來指定要bind到那一個特定的 implementation
- @Scope instance scope
- @Value 通常用來跟屬性值 (property做binding)
- @Lazy bean lazily initialized

stereotype type

- @Component 主要是標明為spring的元件，在auto scan時會被spring處理
- @Repository 同`@Component`，用在DAO上，JPA遇到@Repository時還會做ExceptionTranslation
- @Service  同`@Component`，用在service上
- @Controler 同`@Component`，但多用於 web controller，搭配`RequestMapping`使用 

@JSR250

- @Resource injected by name 而不是 by type
- @PostConstruct 在bean建立後被call back
- @PreDestroy 在bean destroy 前被 call back



JSR 330有另一套Annotations(javax.inject.*)，可以對應到spring，但功能上會比spring專用的annotations有些許限制，如果沒有特別考慮移植性，建議採用spring版的，較能善用spring的功能

<http://static.springsource.org/spring/docs/current/spring-framework-reference/htmlsingle/#beans-standard-annotations>

`@Bean` annotation 也可以使用在 @Component 中，但跟使用在 @Configuration 中會有不同的處理流程

	@Component
	public class FactoryMethodComponent {
	
	  private static int i;
	
	  @Bean @Qualifier("public")
	  public TestBean publicInstance() {
	      return new TestBean("publicInstance");
	  }
	
	  // use of a custom qualifier and autowiring of method parameters
	
	  @Bean
	  protected TestBean protectedInstance(@Qualifier("public") TestBean spouse,
	                                       @Value("#{privateInstance.age}") String country) {
	      TestBean tb = new TestBean("protectedInstance", 1);
	      tb.setSpouse(tb);
	      tb.setCountry(country);
	      return tb;
	  }
	
	  @Bean @Scope(BeanDefinition.SCOPE_SINGLETON)
	  private TestBean privateInstance() {
	      return new TestBean("privateInstance", i++);
	  }
	
	  @Bean @Scope(value = WebApplicationContext.SCOPE_SESSION,
	               proxyMode = ScopedProxyMode.TARGET_CLASS)
	  public TestBean requestScopedInstance() {
	      return new TestBean("requestScopedInstance", 3);
	  }
	}


Resource
--------

注入 resource 時如果沒有指定 prefix (class path, file, http,…)時，會依 context 使用適合的 resource loading 方式，如果有指定 prefix ，就會以 prefix 指定的方式loading resource
	
不指定 prefix 
	
	<bean id="myBean" class="...">
	  <property name="template" value="some/resource/path/myTemplate.txt"/>
	</bean>

指定 prefix
	
	<property name="template" value="classpath:some/resource/path/myTemplate.txt">
	<property name="template" value="file:/some/resource/path/myTemplate.txt"/>
	
	
resource path 可採用 ant-style的 wildcards

     /WEB-INF/*-context.xml
     com/mycompany/**/applicationContext.xml
     file:C:/some/path/*-context.xml
     classpath:com/mycompany/**/applicationContext.xml
     
     
> `classpath*:` 與 `classpath:`的區別
>  `classpath*:` 會在 class path 中找出所有符合條件的 resources
>  `claspath`: 只會在 class path 中找出第一個符合條件的 resource


Validation 
----------

對於非簡單型別的屬性，可以在 Validator 中另外套用其它的 Validator
ex: Customer物件中，基本屬性用CustomerValidator，而 address屬性，不是簡單的資料型別而是物件，可以在CustomerValidator.validate 再套用 address validator

	public class CustomerValidator implements Validator {
	
	    private final Validator addressValidator;
	
	    public CustomerValidator(Validator addressValidator) {
	        if (addressValidator == null) {
	            throw new IllegalArgumentException(
	              "The supplied [Validator] is required and must not be null.");
	        }
	        if (!addressValidator.supports(Address.class)) {
	            throw new IllegalArgumentException(
	              "The supplied [Validator] must support the validation of [Address] instances.");
	        }
	        this.addressValidator = addressValidator;
	    }
	
	    /**
	    * This Validator validates Customer instances, and any subclasses of Customer too
	    */
	    public boolean supports(Class clazz) {
	        return Customer.class.isAssignableFrom(clazz);
	    }
	
	    public void validate(Object target, Errors errors) {
	        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "firstName", "field.required");
	        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "surname", "field.required");
	        Customer customer = (Customer) target;
	        try {
	            errors.pushNestedPath("address");
	            ValidationUtils.invokeValidator(this.addressValidator, customer.getAddress(), errors);
	        } finally {
	            errors.popNestedPath();
	        }
	    }
	}

Type Conversion
---------------
Type Conversion是指格式轉換，通常，常web進來的值，因為都是基於http，所以通常都會用純文字的方式送到後端，
type conversion就是負責把純文字轉成適點的型別或格式。

ex:

	http://locallost/example/user?name=kent&age=10&sex=MALE
	
	class User {
		private String name;
		private int age;
		Sex   sex;
	}
	
	enum Sex {MALE, FEMALE}
	
	
前端進來的是純字串，name="kent"，age="1"，sex="MALE"，但user物件的屬性，age 是int，sex 是enum型別。
type conversion 就是將字串"1"轉成 int 1, 字串 "MALE" 轉成 enum Sex.MALE，當然也可以適用在非字串轉非字串的格式。

spring 提供二種方式可以做type conversion
1. 	PropertyEditors : 這是從java bean那邊借過來的功能，swing也有不少地方用到這個功能
2. 	Conversion SPI  : spring 3後引入的功能，基於泛型(generic)的，所以型別資訊會比較豐富

另外，spring還有 Formatter SPI, 可以做格式化的功能，常用的formatter有

- CurrencyFormatter (org.springframework.format.number)
- NumberFormatter (org.springframework.format.number)
- PercentFormatter (org.springframework.format.number)
- DateFormatter (org.springframework.format.datetime)

或者用 format annotation 

- @DateTimeFormat
- @NumberFormat


如果使用srping mvc的話，可以設定全局性的validator跟controller專用的validator

global validator 

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:mvc="http://www.springframework.org/schema/mvc"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xsi:schemaLocation="
	        http://www.springframework.org/schema/beans
	        http://www.springframework.org/schema/beans/spring-beans.xsd
	        http://www.springframework.org/schema/mvc
	        http://www.springframework.org/schema/mvc/spring-mvc.xsd">
	
	    <mvc:annotation-driven validator="globalValidator"/>
	
	</beans>


controller validator

	@Controller
	public class MyController {
	
	    @InitBinder
	    protected void initBinder(WebDataBinder binder) {
	        binder.setValidator(new FooValidator());
	    }
	
	    @RequestMapping("/foo", method=RequestMethod.POST)
	    public void processFoo(@Valid Foo foo) { ... }
	
	}

@Valid 會觸發 controller 進行 validate 的動作


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


JMS
---
`javax.jms.Exception`是Checked Exception, 使用Spring JmsTemplate則會轉成UncheckedException

spring有提供訊息轉換的功能，可以方便的把java object轉成jms格式的訊息


配值檔

	@Configuration
	public class JmsConfig {
		@Bean
		public JmsTemplate jmsTemplate() {
			final JmsTemplate jmsTemplate = new JmsTemplate(jmsConnectionFactory());
			// jmsTemplate會往名稱為`"my queue`的queue送訊息
			jmsTemplate.setDefaultDestination(new ActiveMQQueue("my queue"));  
			return jmsTemplate;
		}
	
		@Bean
		public ConnectionFactory jmsConnectionFactory() {
			final ActiveMQConnectionFactory factory = new ActiveMQConnectionFactory();
			factory.setBrokerURL("tcp://localhot:61616"); // mq server ip
			return factory;
		}
	
	
		@Bean
		public AbstractJmsListeningContainer jmsContainer() {
			final DefaultMessageListenerContainer container = new DefaultMessageListenerContainer();
			container.setConnectionFactory(jmsConnectionFactory());
			// 當 "my topic" 做訊息發佈時，MyMessageListenerServiceImpl會收到訊息通知
			container.setDestination(new ActiveMQTopic("my topic"));
			//container.setSessionTransacted(true);
			container.setConcurrentConsumers(5);
			container.setReceiveTimeout(10000);
			container.setMessageListener(deviceEventListener());
			return container;
		}
	
	
		@Bean
		public MessageListener deviceEventListener() {
			return new MyMessageListenerServiceImpl();
		}
	}

收/發訊息

收/發訊息會以上面配置檔設定的目的收/發訊息

	@Service
	public class MyMessageListenerImpl implements MessageListener {
	
		private JmsTemplate jmsTemplate;
		@Autowired
		public void setJmsTemplate(JmsTemplate jmsTemplate) {
			this.jmsTemplate = jmsTemplate;
		}
	
		@Test
		public void jsmSend() throws Exception {
			// 往`my queue`發送訊息
			jmsTemplate.convertAndSend("test");
		}
	
		/**
		 * Message Queue上的名稱為"my topic"的topic做publish時會觸發這個method
		 *
		 * @param message
		 */
		@Override
		public void onMessage(Message message) {
			System.out.println(message);
		}
	}

Testing
-------

- org.springframework.test.util.ReflectionTestUtils 提供對private的屬性進行取值、設值的功能以輔助測試
- org.springframework.test.jdbc.JdbcTestUtils 可以有計算 table 內資料筆數的方便的 methods 可以使用


#### Context management and caching ####
spring test context 會進行 caching 以加速測試的速度，cache 是以一個 test suite 為單位，在同一個 jvm 一起執行的所有 test cases 算是同一個 test suite，如果 test suite 被污染時，必須 reload context

`@DirtiesContext` 會把 context 標記為 dirty，被標記為 dirty 後，該 context 會被移出 cache區
