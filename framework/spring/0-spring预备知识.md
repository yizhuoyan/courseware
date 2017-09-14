#Spring预备知识
#一、概述
学习Spring，必须保证下面的知识点已经掌握，否则对某些概念很难理解。
#二、XML、DTD、Schema
##1、XML
可扩展标签语言。主要用于对数据的结构化描述。目前主要的用途是:
  
- 配置文件

	大多数第三库都使用XML作为其配置文件格式，其他配置文件格式还有`ini`，`properties`，`json`，`yaml`等

- 文本数据传输格式

	如AJAX技术中的X就是指XML这种文件格式，类似的文本数据传输格式还有`JSON`，`CSV`等

##2、DTD
文档类型定义。主要用于约束XML文件该如何编写。一般编写XML时需要指定XML文件遵守的约束DTD文件。其定义文件后缀为dtd。如xml引入dtd约束:

~~~xml
<?xml version="1.0"?>
<!DOCTYPE note PUBLIC "http://www.w3schools.com/dtd/note.dtd">
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
~~~

##2、Schema
文档类型定义。也用于约束XML文件该如何编写，功能更加丰富。
其定义文件后缀为xsd。
如xml引入schema:

~~~xml
<?xml version="1.0"?>
<note xmlns="http://www.w3schools.com"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="
http://www.w3schools.com
http://www.w3schools.com/schema/note.xsd">
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
~~~

#三、反射API
JDK中的反射API提供了另一种创建和操作java对象的方法。
##1、通过字符串类名创建此类对象
~~~java
String className="java.util.ArrayList";
Class type=Class.forName(className);
List list=(ArrayList)type.newInstance();
~~~

##2、动态获取对象属性

~~~java
//传入任意对象，返回对应的field属性的值
public Object getFiledValue(Object obj,String field)throws Exception{
		Class type=obj.getClass();
		Field f=type.getField(field);
		return f.get(obj);
}
~~~


##3、动态执行对象方法

~~~java
//执行任意对象的任意方法
public Object invokeMethod(Object obj,String methodName,Object... args)throws Exception{
		Class type=obj.getClass();
		Method method=type.getMethod(methodName, (Class[])Arrays.stream(args).map(v->v.getClass()).toArray());
		return method.invoke(obj, args);
}
~~~

#四、设计模式之单例模式/工厂模式等
#1、单例模式
让某个类仅能被创建一个对象。最简单的写法:

~~~
//饿汉式
public class A{
	private static final A instance=new A();

	private A(){}

	public static A onlyInstance(){
		return instance;
	}
}
~~~

#2、工厂模式
不使用new来创建对象，而是提供一个工厂类，批量生产对象。简单工厂模式的写法:

~~~java
public class Cat{}

public class CatFactory{

	static public Cat get(){
		return createCat();
	}
...
}
public class Test{
	public static void main(String... args){	
		Cat c=CatFactory.get();
		//not Cat c=new Cat();
	}
}
~~~
#五、Log4j
Log4j是Apache下的一个功能非常丰富的Java日志库实现。

使用log4j，仅需在类路径下加入log4j.jar，然后编写log4j.xml/log4j.properties即可。如:

~~~properties
log4j.rootLogger=debug, stdout, R

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
##Pattern to output the caller's file name and line number.
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] (%F:%L) - %m%n

log4j.appender.R=org.apache.log4j.RollingFileAppender
log4j.appender.R.File=example.log

log4j.appender.R.MaxFileSize=100KB
##Keep one backup file
log4j.appender.R.MaxBackupIndex=1

log4j.appender.R.layout=org.apache.log4j.PatternLayout
log4j.appender.R.layout.ConversionPattern=%p %t %c - %m%n
~~~


#六、JCL
JCL(Jakarta Commons Logging)"，也可称为"Apache Commons Logging"。是Apatch组织的下的顶级项目。

JCL不是日志组件，是一种"日志门面"组件。

目前市场上日志有`log4j1`，`log4j2`，`JDK-Logging`,`logback`,`tinylog`等各种日志组件，一个项目要适应不同的日志组件是不可能，这就有了"日志门面"此类组件的用武之地了。

JCL屏蔽不同日志组件的差异(适配器模式)，提供统一的门面(API)。如Spring依赖于JCL来适配不同的日志组件。

同类型的第三方组件还有`slf4j`，比JCL要更优秀。













