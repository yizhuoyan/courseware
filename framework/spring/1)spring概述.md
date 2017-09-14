#一、概述
&emsp;&emsp;spring是一个组织，其官网是<http://spring.io>。

&emsp;&emsp;其下包含多个子项目，如:
spring-framework，spring-boot，spring-cloud，spring-data，spring-mobile，spring-for-android等。

&emsp;&emsp;我们所谓的spring，其实是指spring-framework这个子项目，因为此项目是spring这个组织最早也是使用最多的一个项目。

`注:如果下文没有特指，当我们说spring时，是指spring-framework这个项目`

 
##1、Spring-framework项目
&emsp;&emsp;共20多个模块，其中每个模块对应一个jar文件，如spring-beans.jar则对应beans模块
<img src="asset/spring-overview.png"/>

官方分为6大部分

1. **Core Container(核心容器)**

	包含`spring-beans`，`spring-core`,`spring-context`,`spring-context-support`,`spring-expression` 这5个jar，提供整个spring技术的核心支撑

1. **Aop(面向切面编程)**

	包含
`spring-aop`，`spring-aspects`，`spring-instrument`，`spring-instrument-tomcat`等4个，提供对aop编程的支持，集成AspectJ和对java6中的instrument功能的支持


1. **Data Access/Integration(数据访问/集成)**

	包含`spring-jdbc`, `spring-orm`, `spring-oxm`, `spring-jms` 和`spring-tx`等几个jar，提供对数据访问和集成其他第三方数据访问框架的支持

	`注:其中就包含对hibernate这种ORM框架的集成，我们后面学习的mybatis和spring集成不是spring本身提供的功能，是mybatis官方提供`

1. **Web**

	包含
`spring-web`,`spring-web-mvc`,`spring-websocket` 和 `spring-webmvc-portlet`等jar文件，针对web这块的常用功能的支持

	`注:其中spring-web-mvc就是指我们后面要学习的spring-mvc`

1. **Messaging(消息传递)**
	
	对应`spring-messaging`模块，主要提供对基于消息传递的应用程序的支持

1. Test(测试)

	对应`spring-test`,主要提供对使用junit或TestNG对Spring程序进行单元测试和集成测试等功能

--------------

	`我们目前仅学习核心容器和AOP这两部分内容，其他部分大家以后用到时再去学习`

##2、两个概念(IOC/DI)

IOC(Invertion Of Control 控制反转)/DI(Dependency Injection 依赖注入)

: 一种编程方式，反转对象创建过程中对依赖的处理方式
以前是我们自己控制，现在交给spring进行控制，所以称为控制反转，其处理过程也被称为依赖注入。
**注:两个概念是对同一事物的不同角度的称谓而已，IOC是官方说法**

##3、IOC容器(BeanFactory/ApplicationContext)

&emsp;&emsp;Spring本质上提供一个容器，用于存放和管理某个范围(称为Context)内的所有对象(称为bean)。这个容器提供了控制反转功能，所以又称为IOC容器，由如下4个模块组成:

* Core
* Beans
* Context
* Expression Language

容器对象抽象:

- BeanFactory接口表示底层的容器的抽象

- 子接口ApplicationContext是我们常用的应用容器接口对象

- 针对于Web应用，则使用WebApplicationContext接口对象

- 每个容器对象可具有一个父容器对象

#二、使用流程

##1、添加依赖jar
把依赖jar包放入类路径即可(需要第三方的commong-logging)，后期我们要学会使用maven/gradled等自动化构建工具来导入依赖

	注:spring的每个模块都是独立的一个jar，其命名非常规范，格式为spring-<module>-<version>.jar(eg:spring-bean-4.0.0.RELEASE.jar).

	其中如果后面有-sources为源码，-javadoc为文档


##2、提供配置元数据
spring的配置元数据可以以

0. XML文件
0. Annoation(注解)/java类
0. DSL(Groovy Bean Definition)groovy文件--Spring4新特性 


三种方式提供，但以xml方式最为全面。下面主要讲解xml的写法:
在src下编写spring配置文件，名称随意(eg:spring.xml)
然后在xml中映入默认的beans命名空间。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	 http://www.springframework.org/schema/beans/spring-beans.xsd"
  >

</beans>
~~~
**注:其中的xml约束使用w3c规定的XML Schema，未使用dtd。但spring也支持dtd约束，但不提倡。**






	
 

##3、编写bean类并注册到容器
bean其实就是一个普通的java pojo类或者javabean

~~~java
package com.yizhuoyan.springstudey.demo;
public class BeanA{
	public String doSomething(){
			//....
	}
}
~~~

在spring的配置文件中注册

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans ...省略schema...>
	<bean    id="serviceA"
		  class="com.yizhuoyan.springstudey.demo.BeanA">
	<!--其他依赖注入-->
	</bean>
</beans>
~~~

##4、创建容器
创建容器需要读取到对应的配置信息，根据不同的提供方式使用不同的方法创建

- 配置xml文件在类路径下

	ApplicationContext ac=new ClassPathXmlApplicationContext(资源路径);

- 配置xml文件在文件路径下

	ApplicationContext ac=new FileSystemXmlApplicationContext(资源路径);

- 普通java类加上@Configuration注解提供:

	ApplicationContext ac=new AnnotationConfigApplicationContext(配置类);

	
- groovy文件提供

	ApplicationContext ac=new GenericGroovyApplicationContext(资源路径)


##5、获取bean对象
使用ApplicationContext提供的各种getBean方法获取bean即可
主要有:

- 通过id/name获取
	ac.getBean()
- 通过类型获取
	ac.getBean()
- 通过id/name+类型获取
	ac.getBean

#三、总结

0. 我们使用spring，核心就是容器对象(ApplicationContext)，得到此容器后，我们就可以从中获得任意在容器中的具有的对象
0. 简单理解IOC(控制反转)-我们需要对象时
	- 没用spring的时候

		我们自己控制对象的生成(去new，去创建，去配置等)，控制权在java代码中

	- 使用spring

		我们从spring容器中去取，对象生成的控制权不在java代码中了，控制权反转到spring容器中了，具体体现在配置元数据中了(可以理解为xml中)


0. 有没有感受到工厂模式?












