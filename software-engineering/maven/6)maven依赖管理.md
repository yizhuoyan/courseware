#maven依赖管理

#一、概述
在maven中所有东西都有一个坐标，如maven创建的项目，maven插件，库中的jar等等。称之为artifact，中文翻译为构件。
每个artifact或多或少都会依赖于其他的artifact ，maven对这些依赖提供了统一的管理方式。

#二、依赖的配置
在pom.xml， `<dependencies/>`标签下配置所有的依赖，一个依赖使用一个`<dependency/>`表示。有如下可配置的子标签:

~~~xml
<dependency>
	<groupId/>
    <artifactId/>
    <version/>
    <type/>
    <classifier/>
    <scope/>
    <systemPath/>
    <exclusions/>
    <optional/>
</dependency>
~~~

##1、groupId&artifactId&version
这三个定义了依赖的坐标，是必须定义的。如:

~~~
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
</dependency>
~~~

##2、type
依赖的类型，因为一个artifact可能是一个jar包/war包，也可能是一个项目(pom)或一个插件(maven-plugin)，大对数情况为jar，默认值也为jar。

可配置的值有:

- pom	
- jar	
- maven-plugin	
- ejb	
- war	
- ear	
- rar
- java-source
- javadoc	
- ejb-client	
- test-jar

##3、classifier(分类器)
用于在依赖地址后添加内容进行分类，如xxx-javadoc.jar和
xxx-sources.jar,仅用于`type`为:

- java-source和
- javadoc
- ejb-client
- test-jar

四种情况

##4、scope&systemPath

我们在项目开发过程中依赖的jar包有三种使用场景。

0. 主编译场景

	默认所有依赖的jar必须存放在类路径中，不然无法通过编译。

0. 测试场景

	有些类路径中的jar仅在测试时使用，不需要打包到最终生成物中，如junit这中测试时需要的jar包。

0. 运行场景

	还有些类路径中的jar包不能打包到最终生成物中，如servlet-api.jar,所有的web容器都会提供，如果打包到war中，则会和容器中默认的冲突。

所以scope属性表示使用此依赖的范围，我们有如下值可选:

- compile(默认值)
	在主编译，测试，运行都要使用
- runtime
	表示仅在测试和运行时需要此jar，编译时不需要，如数据库驱动jar。
- test
	表示仅用于测试范围的jar，典型例子就是junit
- provided
此jar在编译和测试时需要，但打包时不需要，由运行环境提供，典型的例子就是servlet-api.api
- system

此jar在编译和测试时需要，但打包不需要，有本地系统提供。
此时不使用maven仓库，指定本地路径。

~~~
<dependency>
		<groupId>com.google.zxing</groupId>
		<artifactId>google-zxing</artifactId>
		<version>1.1.0</version>
		<scope>system</scope>
		<systemPath>${basedir}/src/main/webapp/WEB-INF/lib/google-zxing.jar</systemPath>
</dependency>
~~~

- import(暂略)
用于引入`<dependencyManagement>`定义的依赖。

##5、optional(暂略)
用于指定依赖传递中是否必须。见依赖传递管理章节。
##6、execlusions(暂略)
用于指定依赖传递过程中的排除。见依赖传递管理章节。

