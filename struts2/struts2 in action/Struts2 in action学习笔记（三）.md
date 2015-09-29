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