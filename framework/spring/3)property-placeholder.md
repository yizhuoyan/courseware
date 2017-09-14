#property-placeholder
#一、概述
有时我们会希望把系统中的所有的相关配置放到一起，便于后期的维护部署，如数据库连接，文件上传默认保存路径等之类的信息。那么这些数据如何用到spring容器中呢?

Spring在`context`命名空间下增加了一个`<context:property-placeholder />`标签，用于加载一个properties文件。可让文件中的配置用在spring中。

#二、配置源
引入命名context空间，然后使用`<context:property-placeholder />`标签使用location属性指定对应的properties文件路径即可。

如:有文件jdbc.properties

~~~properties
jdbc.driverClassName=org.hsqldb.jdbcDriver
jdbc.url=jdbc:hsqldb:hsql://production:9002
jdbc.username=sa
jdbc.password=root
~~~

导入

~~~xml
...
<context:property-placeholder location="classpath:com/foo/jdbc.properties"/>
...
~~~

**注:**property-placeholder不仅仅导入我们配置的properties文件内容， 默认其会导入`System.getProperties()`返回的系统属性。
#三、在配置文件中使用
在配置文件使用${表达式}引用即可。

~~~xml
<bean id="dataSource" destroy-method="close"
        class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driverClassName}"/>
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>
~~~

#四、在java类中使用
在java类中，使用`@Value`注解引用其值赋值给属性，方法参数等。

~~~java
public class MyBean {

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;
}
~~~
**注:`@Value`注解的功能不仅如此，可在其中使用spEL表达式**

#总结

使用property-placeholder方式可以让我们的配置更加方便。