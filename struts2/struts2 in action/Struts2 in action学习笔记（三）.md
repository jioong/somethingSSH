# Struts2 Action 动作 #
  
动作完成每一个请求的核心处理，包括业务逻辑、承载数据，之后选择应该呈现结果页面的结果。 *Struts2* 是一个面向动作(*action-oriented*) 的框架，动作是这个框架的核心。  
  
## Struts2 动作简介 ##
  
**动作的作用：**  
1. 为给定请求封装需要做的实际工作。  
2. 在从请求到视图的框架自动数据传输中作为数据的携带者。  
3. 动作必须帮助框架决定哪个结果应该呈现请求响应中返回的视图。  
  
**动作封装工作单元**  
动作使用 *execute()* 方法来实现控制业务逻辑的职责。该方法只关注于与请求相关的工作逻辑。  
```Java
public String execute() {
	setCustomGreeting( GREETING + getName());
	return "SUCCESS";
}
```  
**动作为数据转移提供场所**  
动作是框架的模型组件，也意味着**动作必须能够携带数据。**数据保存在动作本地，在执行业务逻辑的过程中可以很方便地访问到它们。动作只需把每一个期望承载的数据实现为 *JavaBean* 属性。  
```Java
将数据请求转移到动作的 JavaBean 属性上
private Sting name;

public String getName() {
	return name;
}  

public void setName( String name){
	this.name = name;
}  
```  

**动作为结果路由选择返回控制字符串**  
动作组件最后一个职责是返回控制字符串用以选择应该被呈现的结果页面。返回字符串的值必须与配置在声明性架构中期望的结果的名字匹配。  
例如， *HelloWorldAction* 返回 *SUCCESS* 字符串，可以从 *XML* 配置文件中看出， *SUCCESS* 是某一个结果组件的名字。  
```XML
<action name="HelloWorld" class="manning.HelloWorld">
	<result name="SUCCESS">/chapterTwo/HelloWorld.jsp</result>
	<result name="ERROR">/chapterTwo/Error.jsp</result>
</action>
```
大部分现实世界中的动作会有更复杂的决策过程，可选结果中总会包含一些错误结果来处理动作与模型交互过程中的错误。但是，不管多么复杂，**动作最终必须返回一个字符串，用来指向一个能够为这个动作呈现视图的可用的结果组件。**  
  
## 打包动作 ##
  
不管是使用 *XML* 的方式声明动作组件，还是使用 *Java* 注解的方式声明动作组件，当框架创建应用程序的架构时，架构会把这些动作组件和其他组件一起放在一起放在一种叫做**包(package)**的逻辑容器内。 *Struts2* 的包很像 *Java* 包，它提供了一种基于功能或者领域的共性将动作组件分组的机制。  
  
*Struts2* 包的属性：  
* *Name*（必须） --- 包的名字  
* *namespace*   --- 包内所有动作的命名空间  
* *extends*     --- 被继承的父包  
* *abstract*    --- 如果为 *true*,这个包只能用来定义可继承的组件，不能定义动作  

可以为不同的包设置相同的命名空间。如果这样的话，两个包中的动作会映射到相同的命名空间。当不设置 *namespace* 属性时，动作会进入默认命名空间。默认命名空间在所有其他命名空间之下，用来解决不能与任何显示声明的命名空间匹配的请求。  
**默认命名空**间实际上是空字符串“ ”。同时，可以定义叫做 *"/"* 的**根命名空间**。根命名空间与其他显示声明的命名空间相同，必须本完全匹配。区分空的默认命名空间和根命名空间非常中央。在空的默认命名空间中，动作名字匹配的所有请求都可以被补货，而在根命名空间是一个真实的命名空间，必须要完全匹配。  
  
### 使用 struts-default 包中的组件 ###
  
使用 *struts-default* 包中的组件很容易，只需要在创建自己的包时扩展该默认包即可。  
  
> 定义：*Struts-default* 包定义在系统的 *struts-default.xml* 文件中，声明了大量常用的 *Struts2* 组件，从完整的拦截器栈到常用的结果类型。  

## 实现动作 ##
  
基本上，如果愿意，任何类都可以作为一个动作。只需要提供一个工作被执行时框架调用的入口方法。  
  
> *Struts2* 动作不必实现 *Action* 接口。任何对象都可以通过实现一个返回控制字符串的 *execute()* 方法来非正式地实现动作与框架之间的契约。  

### 可选的 Action 接口 ###
  
框架没有对动作强加很多正式的要求，但是它确实提供了一个可以选择实现的接口。实现 *Action* 接口的成本很小，并且带来了一些方便。  
大部分的动作会实现 *com.opensymphony.xwork2.Action* 接口，该接口只定义了一个方法：  
```Java
	String execute() throws Exception
```    

由于框架没有任何类型要求，所以可以不实现该接口，而把这个方法直接放在类中。 *Action* 接口同时也提供了一些有用的 *String* 常量，这些常量可以作为返回值来选择合适的结果。 *Action* 接口定义的常量为：  
```Java
public static final String ERROR   "error"
public static final String INPUT   "input"
public static final String LOGIN   "login"
public static final String NONE    "none"
public static final String SUCCESS "success"
```
  
这些常量可以很方便的用作 *execute()* 方法返回的控制字符串的值。真正的好处是框架内部也是用了这些常量，这意味着使用这些预定义的控制字符串允许接入更多的智能默认行为。  
  
### ActionSupport 类 ###
  
它提供了 *Action* 接口和其他几个有用接口的默认实现的便利类，提供了注入数据验证、错误消息本地化等功能。  
  
**1. 基本验证**  
