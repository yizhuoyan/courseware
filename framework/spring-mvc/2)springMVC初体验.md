#springMVC初体验
#一、概述
**Springmvc**是`Spring Framework`项目下的`Web`部分，主要包含`spring-web`, `spring-webmvc`, `spring-websocket`和`spring-webmvc-portlet`等几个模块。

整个springMVC是构建在Spring的`IOC`容器上的，使用`WebApplicationContext`作为容器对象。


#二、步骤
##1、添加依赖jar

无构建工具:
: 直接添加spring相关jar包到类路径

maven:
: 
~~~xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.3.7.RELEASE</version>
</dependency>
~~~

gradle:
: 
~~~gradle
dependencies {
    compile 'org.springframework:spring-context:4.3.7.RELEASE'
}
~~~

##2、编写spring-web配置文件

在项目`根路径`下编写spring-mvc.xml配置文件，并引入`mvc`命名空间且激活mvc注解驱动`<mvc:annotation-driven>`

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc.xsd
	">	
	<mvc:annotation-driven></mvc:annotation-driven>
</beans>
~~~

##3、配置web.xml
在web.xml中配置spring提供的一个Servlet，进行spring-mvc容器的创建。

~~~xml
  <servlet>
        <servlet-name>mvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
			<description>配置springmvc的配置文件的地址</description>
        	<param-name>contextConfigLocation</param-name>
        	<param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
		<!--此servlet随着服务器启动-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>mvc</servlet-name>
		<!--定义springmvc可以处理的url模式，这里表示所有.html结尾的请求-->
        <url-pattern>*.html</url-pattern>
    </servlet-mapping>
~~~

##4、编写handler

编写处理请求的handle，使用`@Controller`用于扫描注册，
使用`@RequestMapping("/login.html")`进行请求映射。

~~~java
@Controller
public class UserHandler {

	@RequestMapping("/login.html")
	public String login(String account,String password){
		return "/index.jsp";
	} 
}
~~~

其中方法的返回值作为视图的名称，`/index.jsp`必须存在。


##5、编写视图index.jsp

~~~
	你好!
~~~

##6、扫描handel进行注册
在`spring-mvc.xml`中引入`context`命名空间，扫描handler所在包进行注册。

~~~xml
<context:component-scan base-package="handler"/>
~~~

##6、启动服务器并测试

启动服务器后，打开浏览器输入`http://.../应用名称/login.html`，如果输出`你好!`，这表示成功!


#总结


