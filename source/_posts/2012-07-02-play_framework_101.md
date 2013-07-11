---
author: Kent Chiu
published: true
layout: post
title: "play framework 101"
date: 2012-07-02
comments: true
external-url:
sharing: true
footer: true
categories:
  - play_framework
  - scala
---



### 在eclipse直接執行play console

### Http Request處理流程

```
    browser : http://localhost/foo?bar=baz

    # project_home/conf/routes
    GET    /foo                        controllers.Application.foo(bar: String)

    # project_home/app/controllers/Application.scala
    object Application extends Controller {

        def foo(bar: String) = Action {
            Ok("bar param value is: " +  bar) // 網頁顯示 bar param value is: baz
        }
    }

```

1.  由網頁發起url: http://localhost/foo?bar=baz
2.  play收到url後，會由routes這個設定檔來決定負責處理的controller是誰
3.  controller中會有一個跟url
    path相符的method進行處理，然後回應一個Action

主要的流程就這樣，相當的單純

### Controller

Controller是一個Singlton
Object，用來產生Action

### Action

在play.api.mvc裡有兩個Action，一個是Singlton Object,另一個是trait，
trait的這個Action是一個 `play.api.mvc.Request ⇒ play.api.mvc.Result`
function負責處理並回應http request， 而Singtlton
Object這個Action是companion object，提供建立Action的一些helper
methods

Ok()這個method會回http status code 200給browser，http status code
200，就是沒有任何錯誤

![play_framework_101-001.png][play_framework_101-001.png]

上面有提到Action是`play.api.mvc.Request ⇒ play.api.mvc.Result`
function，而AsyncResult, ChunkedResult, PlainResult, SimpleResult,
Status都是extends自trait Result，所以可以當作Action的傳回值

```
    package controllers
     
    import play.api._
    import play.api.mvc._
     
    object Application extends Controller {
     
      def index = Action {
        Ok(views.html.index("Your new application is ready."))
      }
     
    }
```

Ok(context) 表示回應http 200(正常)給網頁，content是要顯示的內容

ex:

`Ok("Hello World") `

網頁就會顯示 Hello World

`views.html.index("Your new application is ready.") `

則是套用play的template來顯示內容

```
    def index(param: String) = Action { implicit request =>
      Ok("Got request [" + request + "] with param [" + param +"]")
    }
```

### Request and Session

Play的Session可以透過request.session取得，它是用cookie實作的，而且沒有所謂的session
timeout，如果需要session timeout的功能，要自已實作。

Play的Request不像java那樣，可以直接當作資料的載體，需透過Flash
Scope(也就是java servlet的request scope)

Scala Template Engine
----

#### Handling the form submission

在todolist這個教學中，有一段code是在建立 new
task的，這一些，對非scala的熟手來說不是那麼的易懂

```
    // Application
    def newTask = Action { implicit request =>
      taskForm.bindFromRequest.fold(
        errors => BadRequest(views.html.index(Task.all(), errors)),
        label => {
          Task.create(label)
          Redirect(routes.Application.tasks)
        }
      )
    }
```

newTask這個method的傳回值是Action型別,
taskForm一是個`play.api.data.Form`型別，而`bindFromRequest`跟`fold`的API如下:
taskForm.bindFromRequest的回傳值是Form[String]，而newTask的型別是Action，所以taskForm.bindFromRequest的結果不能直接當newAction的結果。
fold的回傳值是R，我們只要讓R為Action型別即可當newTask的回傳值。

```
    /**
     * Binds request data to this form, i.e. handles form submission.
     *
     * @return a copy of this form filled with the new data
     */
    def bindFromRequest()(implicit request: play.api.mvc.Request[_]): Form[T] = {
      val data = (request.body match {
        case body: play.api.mvc.AnyContent if body.asFormUrlEncoded.isDefined => body.asFormUrlEncoded.get
        case body: play.api.mvc.AnyContent if body.asMultipartFormData.isDefined => body.asMultipartFormData.get.asFormUrlEncoded
        case body: play.api.mvc.AnyContent if body.asJson.isDefined => FormUtils.fromJson(js = body.asJson.get).mapValues(Seq(_))
        case body: Map[_, _] => body.asInstanceOf[Map[String, Seq[String]]]
        case body: play.api.mvc.MultipartFormData[_] => body.asFormUrlEncoded
        case body: play.api.libs.json.JsValue => FormUtils.fromJson(js = body).mapValues(Seq(_))
        case _ => Map.empty[String, Seq[String]]
      }) ++ request.queryString
      bind(data.mapValues(_.headOption.getOrElse("")))
    }
   
   
    def fold[R](hasErrors: Form[T] => R, success: T => R): R = value.map(success(_)).getOrElse(hasErrors(this))
```

fold的第一個hasErrors的參數是，在這邊是個像這樣的function literal
Form[String] ⇒ R(也就是你需要傳入適合的function value進去)
因為我們希望fold的結果可以直接當newTask
method的回傳值，所以R的型別為Action，也就是

```
    def fold[R](hasErrors: Form[T] => R, success: T => R): R = value.map(success(_)).getOrElse(hasErrors(this))
```

在這個例子的實際的型別是

```
    def fold[Action](hasErrors: Form[String] => Action, success: String => Action): Action = value.map(success(_)).getOrElse(hasErrors(this))
```

fold的使用範例是這樣

```
        anyForm.bindFromRequest().fold(
           f => redisplayForm(f),                   // hasErrors: Form[String] => Action
           t => handleValidFormSubmission(t)        // success: String => Action 
        )

```

裡面的redisplayForm跟handleValidFormSubmission並不是play
API的一部份，他只是一個pseudo
code，用來表示通常hasErrors的處理方式是redisplayForm，而success的處理方式是handleValidFormSubmission

再回來看成來的例子

```
    // Application
    def newTask = Action { implicit request =>
      taskForm.bindFromRequest.fold(
        errors => BadRequest(views.html.index(Task.all(), errors)),  // hasErrors: Form[String] => Action
        label => {                                                   // success: String => Action 
          Task.create(label)
          Redirect(routes.Application.tasks)
        }
      )
    }
```

##### hasErrors參數的部份

errors是Form[String]，而BadRequest是Action型別

hasErrors時會redisaply form，也就是用BadRequest(status 400)取代Ok(status
200)呼叫views.html.index，並把form的內容傳入

##### success參數的部份

label是String型別,
而傳回值Redirect(routes.Application.tasks)是Action型別 success時會handle
valid form submission，也就是建立task後，然後再轉到task list的頁面

MISC
====

### mapping method in Form

```
import play.api.data._
import play.api.data.Forms._
 
case class User(name: String, age: Int)
 
val userForm = Form(
  mapping(
    "name" -> text,
    "age" -> number
  )(User.apply)(User.unapply)
)
 
val anyData = Map("name" -> "bob", "age" -> "18")
val user: User = userForm.bind(anyData).get
```

Form裡面的mapping，對於play的新手，很容易誤為是Form.mapping()，但怎麼看，都對不起來，完全不知道為什麼可以用apply()跟unapply()，其實它是Forms.mapping()

Forms.mapping是長這樣

```
    /**
     * Creates a Mapping of type `T`.
     *
     * For example:
     *
     * @tparam T the mapped type
     * @param apply A function able to create a value of T from a value of A1 (If T is case class you can use its own apply function)
     * @param unapply A function able to create A1 from a value of T (If T is a case class you can use its own unapply function)
     * @return a mapping for type `T`
     */
    def mapping[R, A1](a1: (String, Mapping[A1]))(apply: Function1[A1, R])(unapply: Function1[R, Option[(A1)]]): Mapping[R] = {
      ObjectMapping1(apply, unapply, a1)
    }
```

對於本來就看不太懂API的新手，還找錯API，只會讓新手更覺得太多無法理解的migic

#### 細節說明

```
mapping[R, A1](a1: (String, Mapping[A1]))(apply: Function1[A1, R])(unapply: Function1[R, Option[(A1)]]): Mapping[R]
```

mapping(…)(…)(…)把參數分成多個”群組”，表示這個function是_curried(柯里化)_

先做大部拆解，這個method可以分為五個部份

1.  mapping[R, A1] 名稱，對應到上例中的mapping
2.  (a1: (String, Mapping[A1])) 參數第一組，對應到上例中的 “email” →
    of[String]
3.  (apply: Function1[A1, R]) 參數第二組，對應到上例中的 User.apply
4.  (unapply: Function1[R, Option[(A1)]]) 參數第三組，對應到上例中的
    User.unapply
5.  Mapping[R] 回傳值

apply,unapply是來自case class

##### mapping[R, A1]

這部份可以知道裡面會有兩個泛型，一個是R,一個是A1

##### (a1: (String, Mapping[A1]))

`(String, Mapping[A1])`是一個tuple，本例中為`“email” → of[String]`

Mapping[A1]中的Mapping是trait型別，主要的功能是做格式間的轉換，本例是將request
string透過轉成String,Int,…格式

##### (apply: Function1[A1, R])

`(apply: Function1[A1, R])`是一個function literal,在本例中為`User.apply`，

作用是將mapping的結果轉成User物件

由API可以知道Mapping是泛型`Mapping[R]`，所以R在例中，是`User`

##### (unapply: Function1[R, Option[(A1)]])

`(unapply: Function1[R, Option[(A1)]])`是一個function
literal,本例中為`User.unapply`

作用是將User物件解構，把User的值再指派到`“email” → of[String]`

[play_framework_101-001.png]: /images/wiki/scala/play_framework_101-001.png