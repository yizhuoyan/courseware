#Maven概述
#一、Maven是什么?

Maven是一个java项目开发中的辅助工具，主要解决3个问题。

0. 项目结构
0. 项目构建
0. 依赖管理

#二、项目结构
我们开发一个javaweb项目，必然需要如下目录:

- 源码目录(存放java代码)
- 资源文件目录(如配置文件，初始化sql等)
- 测试代码目录(存放测试代码)
- web页面目录(存放jsp，html,js等)
- 图片目录

那么这些目录如何组织和命名呢?

maven提供了不同类型项目默认的命名规则和结构，如果所有项目都遵守统一的约定，则项目构建能减少很多配置问题。

maven把统一的项目结构称为项目原型(archetype)。其默认提供了不同项目的结构原型。

#三、项目构建
我们开发一个java项目大体流程如下:

0. 搭建开发环境
0. 代码编写
0. 编译后单元测试
0. bug修改(多次)
0. 整体测试后打包发布一个版本或到站点
0. 新的需求或bug修改(多次)
0. 编译后单元测试
0. 整体测试后发布一个版本
0. ....无穷迭代上面三个步骤

如果每个项目都需要这些流程，那么可不可抽取出一个统一的项目构建流程然后用工具来自动完成呢?

这就是Maven提供的项目构建功能做的事。

**注:Maven之前项目构建主要使用Make和Ant**

#四、依赖管理

java的第三方库都是以`jar`的形式提供，一般一个第三方库都有多个jar而且可能还依赖与其他第三方库，这就会形成一个依赖链。在某些时候甚至会发生版本冲突。如下面的开发场景问题:

- `下载繁琐`:我的项目需要使用spring+springMVC+mybatis，则我需要分别去各个官网下载各自提供的jar包，而这个库还依赖于其他库的jar，也需要我去判断依赖版本专门下载特定版本。。。

- `版本冲突`:如我的项目依赖A库和B库。然后A库依赖a-1.0.jar,b-1.0.jar,而B库依赖于a-2.0.jar,这时候就会存在版本冲突。

Maven提供了中央库和依赖坐标的概念。

##1、依赖仓库(Repository):
仓库上有所有的jar包，当项目构建中需要添加jar时，则直接从仓库中下载即可，解决下载繁琐的问题。maven官方提供了默认的中央(Central)仓库，很多第三方也提供了免费的仓库，如果有需要的话，公司内部也可以搭建局域网仓库。



##2、依赖坐标(Dependency Coordinate)
每一个jar包在仓库中都有一个坐标来进行定位，如以下配置:

~~~xml
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.3.7.RELEASE</version>
</dependency>
~~~

- groupId:一般是组织或机构名,如果大项目可加上项目名
- artifactId:项目名，如果大项目此处为模块名
- version:版本

如上面的坐标就指定了如何来获取spring-webmvc这个jar包。路径为`仓库路径/org.springframework/spring-webmvc/4.3.7.RELEASE/spring-webmvc-4.3.7.RELEASE.jar`。

而且maven自动管理依赖链，如上面配置，项目中实际会添加如下加入包。

~~~
spring-webmvc-4.3.7.RELEASE.jar
spring-aop-4.3.7.RELEASE.jar
spring-beans-4.3.7.RELEASE.jar
spring-context-4.3.7.RELEASE.jar
spring-core-4.3.7.RELEASE.jar
commons-logging-1.2.jar
spring-expression-4.3.7.RELEASE.jar
spring-web-4.3.7.RELEASE.jar
~~~

#总结
我们在项目开发时可以不使用maven，但使用maven能让我们的开发过程更加便捷和规范。
