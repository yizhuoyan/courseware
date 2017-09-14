#AJAX支持

#一、概述
SpringMVC对AJAX的支持就是指应答可以返回JSON/JSONP的格式的数据。

#二、返回JSON格式数据

##1、加入jackson相关jar
- MAVEN

~~~xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-core</artifactId>
	<version>2.8.1</version>
</dependency>
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.8.1</version>
</dependency>
~~~
- GRADLE

~~~gradle
compile group: 'com.fasterxml.jackson.core', name: 'jackson-core', version: '2.8.1'
compile group: 'com.fasterxml.jackson.core', name: 'jackson-databind', version: '2.8.1'
~~~

- 手动

添加

- jackson-annotations.jar
- jackson-core-2.8.1.jar
- jackson-databind-2.8.1.jar

三个jar包到类路径。


##二、注册HttpMessageConverter

注册支持JSON/JSONP格式的HttpMessageConverter

~~~xml
<mvc:annotation-driven>
	<mvc:message-converters register-defaults="false">
       <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
	     <property name="defaultCharset" value="utf-8"></property>
	     <property name="prettyPrint" value="true"></property>
		</bean>
  </mvc:message-converters>
<mvc:annotation-driven>
~~~

##3、handler方法使用@RespondBody注解
把@ResponseBody注解在方法，表示此方法的返回值将使用对应的HttpMessageConverter准换后输出为应答体。

~~~java
public class MyHandler {
	
	@RequestMapping(path="/test")
	@ResponseBody
	public String[] test(String account){
		return new String[]{"你大爷","abc"};
	}
~~~

##4、测试
客户端请求:

~~~
..../test
~~~

客户端接收:

- 应答头

~~~
Content-Type:application/json;charset=UTF-8
~~~

- 应答体

~~~
[ "你大爷", "abc" ]
~~~

#三、返回JSONP数据
所有配置和返回JSON数据一致，多一个配置回调方法名的过程。

##1、配置JSONP返回
使用`@ControllerAdvice`编写一个全局配置Controller的类，在构建中指定回调方法列表。

~~~java
@ControllerAdvice
public class JsonpAdvice extends AbstractJsonpResponseBodyAdvice {
    public JsonpAdvice() {
        super("jsonp","callback","cb");
    }
}
~~~

##2、测试
客户端请求:

~~~
..../test?jsonp=onDone
~~~

客户端接收:

- 应答头

~~~
Content-Type:application/javascript
~~~

- 应答体

~~~
/**/onDone([ "你大爷", "abc" ]);
~~~

#总结


