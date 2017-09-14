#maven的项目构建生命周期
#一、概述
项目构建的生命周期无外乎有如下阶段:

0. 构建前的清理工作
0. 初始化(资源，环境变量等准备)
0. 编译
0. 测试(单元测试等)
0. 打包(打成jar/war等)
0. 集成测试(各个模块整体测试)
0. 发布版本(生成文档，手册，案例等之类)
0. 生成站点(可选)

maven总结了一套适用于大多数项目的，易扩展的项目构建生命周期。

maven仅抽象的生命周期各个阶段，每个阶段具体的任务(goal)则有不同的`插件(plugin)`来完成，如编译阶段使用`maven-compiler-plugin`插件的`compile`goal来完成的。

#二、三套生命周期
maven有三套完成独立的生命周期

- clean
	用于清理
- default
	用于构建
- site
	用于站点生成

每套生命周期都包含多个有序的阶段。执行某个阶段，则此阶段前面的阶段也会执行。



##1、clean
主要用于清理上一次构建的输出。包含三个阶段:

0. pre-clean 清理前
0. clean 清理阶段
0. post-clean 清理后

##2、site
主要用于建立项目站点信息。包含四个阶段:

0. pre-site 生成前
0. site 生成站点文档
0. post-site 生成后
0. site-deploy 发布站点

##3、default
maven中最重要也是最复杂的生命周期，包含23个阶段，下面主要介绍其中常用的阶段。

0. validate  验证项目配置是否正确
0. compile  编译
0. test  进行测试
0. package  打包
0. deploy  发布

#三、项目构建

当我们在名某个项目下运行命令`mvn clean deploy`时，会先执行clean生命周期的pre-clean和clean两个阶段(不会执行post-clean)

，然后执行默认生命周期中的所有23个阶段(因为deploy是最后一个阶段)。

#四、一个示例
##1、实用简单的项目原型创建
命令:`mvn archetype:generate`

选择7 maven-archetype-quickstart

~~~
...
Choose a number or apply filter (format: [groupId:]artifactId, case sensitive co
ntains): 7:
~~~

##2、输入坐标并确认

~~~
...
Define value for property 'groupId': com.yizhuoyan.studymaven
Define value for property 'artifactId': lifecycle
Define value for property 'version' 1.0-SNAPSHOT: :
Define value for property 'package' com.yizhuoyan.studymaven: :
Confirm properties configuration:
groupId: com.yizhuoyan.studymaven
artifactId: lifecycle
version: 1.0-SNAPSHOT
package: com.yizhuoyan.studymaven
 Y: :
~~~

##3、项目结构一览

默认结构如下:

~~~
C:\USERS\ADMINISTRATOR\LIFECYCLE
│  pom.xml
│
└─src
    ├─main
    │  └─java
    │      └─com
    │          └─yizhuoyan
    │              └─studymaven
    │                      App.java
    │
    └─test
        └─java
            └─com
                └─yizhuoyan
                    └─studymaven
                            AppTest.java
~~~

##4、App.java源码

~~~java
package com.yizhuoyan.studymaven;

/**
 * Hello world!
 *
 */
public class App{
    public static void main( String[] args ){
        System.out.println( "Hello World!" );
    }
}

~~~

##5、AppTest.java源码

~~~java
package com.yizhuoyan.studymaven;

import junit.framework.Test;
import junit.framework.TestCase;
import junit.framework.TestSuite;

/**
 * Unit test for simple App.
 */
public class AppTest  extends TestCase{
    /**
     * Create the test case
     *
     * @param testName name of the test case
     */
    public AppTest( String testName ){
        super( testName );
    }

    /**
     * @return the suite of tests being tested
     */
    public static Test suite(){
        return new TestSuite( AppTest.class );
    }

    /**
     * Rigourous Test :-)
     */
    public void testApp(){
        assertTrue( true );
    }
}
~~~
##6、配置打包插件，添加manifest信息
编辑项目下的POM.xml,添加如下信息:

~~~xml
  <build>
	<plugins>
		<plugin>
			<artifactId>maven-jar-plugin</artifactId>
			<configuration>
				<archive>
					<manifest>
						<mainClass>com.yizhuoyan.studymaven.App</mainClass>
					</manifest>
				</archive>
			</configuration>
		</plugin>
    </plugins>
  </build>
~~~

主要是配置manifest信息，便于打包为可执行jar。

##7、执行打包
使用命令
`mvn clean package`,输出如下:

清理阶段

~~~console
[INFO] ------------------------------------------------------------------------
[INFO] Building lifecycle 1.0-SNAPSHOT
[INFO]------------------------------------------------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ lifecycle ---
[INFO] Deleting C:\Users\Administrator\lifecycle\target
~~~

资源准备阶段(跳过):

~~~
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ lifecycle---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\Users\Administrator\lifecycle\src\main\resources
~~~

编译源码阶段

~~~
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ lifecycle ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\Users\Administrator\lifecycle\target\classes
~~~

测试资源准备阶段(跳过)

~~~
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ lifecycle ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory C:\Users\Administrator\lifecycle\src\test\resources
~~~

编译测试代码阶段

~~~
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ lifecycle ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to C:\Users\Administrator\lifecycle\target\test-classes
~~~

执行测试阶段

~~~
[INFO] --- maven-surefire-plugin:2.12.4:test (default-test) @ lifecycle ---
[INFO] Surefire report directory: C:\Users\Administrator\lifecycle\target\surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.yizhuoyan.studymaven.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.009 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
~~~

打包阶段:

~~~
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ lifecycle ---
[INFO] Building jar: C:\Users\Administrator\lifecycle\target\lifecycle-1.0-SNAPSHOT.jar
~~~

最后任务执行结果:

~~~
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.440 s
[INFO] Finished at: 2017-04-24T11:54:25+08:00
[INFO] Final Memory: 16M/161M
[INFO] ------------------------------------------------------------------------
~~~
##7、执行

输入java执行jar命令

~~~
C:\Users\Administrator\lifecycle>java -jar target\lifecycle-1.0-SNAPSHOT.jar
Hello World!
~~~











