#Spring表达式语言
#一、概述
Spring表达式语言(Spring Expression Language)，简称为spEL。

一种表达式语言，类似于EL，但功能更加强大，主要用于获取或设置对象属性或执行方法等。

可用于xml配置文件或`@Value`注解
#二、基本使用
其语法是#{表达式}，注意和`property-placeholder`的语法区别(后者是$)

##1、xml文件中使用
~~~xml
<bean id="a" class="...">
	<property name="age" value="8"></property>
</bean>
<bean class="...MyBean">
    <property name="someAge" value="#{a.age}"/>
</bean>
~~~

##2、@Value注解

~~~java
public class MyBean

  @Value("#{systemProperties['user.region']}")
  private String defaultLocale;
...
}
~~~

#三、语法

spEL的语法和EL类似，主要是使用.和[]来访问对象设置属性
其他语法的不同处和高级用法请查阅相关资料。

**注:spEL主要用于spring内部，项目开发工作中用的较少。**



