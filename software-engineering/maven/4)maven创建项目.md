#maven创建项目
#一、概述

maven能解决的第一个问题就是统一项目结构，下面来进行介绍。

#二、步骤
##1、输入命令
`mvn archetype:generate`，此命令表示执行archetype这个插件中的generate功能。

如果是第一次使用，此时会有巨量的下载发生(下载执行archetype此插件所需的jar)，下载完成后，会询问创建哪种类型的项目。

##2、选择项目原型
maven内置了很多项目原型，提示我们按照序号进行选择:

~~~
1: internal -> org.apache.maven.archetypes:maven-archetype-archetype (An archetype which contains a sample archetype.)
2: internal -> org.apache.maven.archetypes:maven-archetype-j2ee-simple (An archetype which contains a simplifed sample J2EE application.)
3: internal -> org.apache.maven.archetypes:maven-archetype-plugin (An archetypewhich contains a sample Maven plugin.)
4: internal -> org.apache.maven.archetypes:maven-archetype-plugin-site (An archetype which contains a sample Maven plugin site.     This archetype can be layered upon an existing Maven plugin project.)
5: internal -> org.apache.maven.archetypes:maven-archetype-portlet (An archetype which contains a sample JSR-268 Portlet.)
6: internal -> org.apache.maven.archetypes:maven-archetype-profiles ()
7: internal -> org.apache.maven.archetypes:maven-archetype-quickstart (An archetype which contains a sample Maven project.)
8: internal -> org.apache.maven.archetypes:maven-archetype-site (An archetype which contains a sample Maven site which demonstrates  some of the supported document types like APT, XDoc, and FML and demonstrates how to i18n your site. This archetype can be layered upon an existing Maven project.)
9: internal -> org.apache.maven.archetypes:maven-archetype-site-simple (An archetype which contains a sample Maven site.)
10: internal -> org.apache.maven.archetypes:maven-archetype-webapp (An archetype which contains a sample Maven Webapp project.)

Choose a number or apply filter 
(format: [groupId:]artifactId, case sensitive contains): 7: 

~~~
我们开发web项目，一般选择`maven-archetype-webapp`，也就是内置的第10个。

##3、输入当前项目的坐标
我们创建的项目在maven的环境中也必须具有一个唯一的坐标。

根据提示依次输入组织id，构件id，版本等信息。如果不输入直接回车，则使用默认值。

~~~
Define value for property 'groupId': com.yizhuoyan.studymaven<回车>
Define value for property 'artifactId': archetype<回车>
Define value for property 'version' 1.0-SNAPSHOT: :<回车>
Define value for property 'package' com.yizhuoyan.studymaven: :<回车>
Confirm properties configuration:
groupId: com.yizhuoyan.studymaven
artifactId: archetype
version: 1.0-SNAPSHOT
package: com.yizhuoyan.studymaven
 Y: :<回车>
~~~
##4、确认

最后会有一个确认。直接回车确认。输出成功信息如下:

~~~
[INFO] -------------------------------------------------------------------------
---
[INFO] Using following parameters for creating project from Old (1.x) Archetype:
 maven-archetype-j2ee-simple:1.0
[INFO] -------------------------------------------------------------------------
---
[INFO] Parameter: basedir, Value: C:\Users\Administrator
[INFO] Parameter: package, Value: com.yizhuoyan.studymaven
[INFO] Parameter: groupId, Value: com.yizhuoyan.studymaven
[INFO] Parameter: artifactId, Value: archetype
[INFO] Parameter: packageName, Value: com.yizhuoyan.studymaven
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] project created from Old (1.x) Archetype in dir: C:\Users\Administrator\a
rchetype
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 12:50 min
[INFO] Finished at: 2017-04-24T10:39:44+08:00
[INFO] Final Memory: 17M/169M
[INFO] ------------------------------------------------------------------------
~~~

##5、查看
在输出目录`C:\Users\Administrator\archetype`具有如下的目录结构。

~~~
C:\Users\Administrator\archetype\
│  pom.xml maven项目配置文件
│
└─src
    └─main
        ├─resources 资源目录
        └─webapp  web代码目录
            │  index.jsp
            │
            └─WEB-INF 
                    web.xml
~~~

每个maven项目根目录下都有一个pom.xml文件，称为(project object model 项目对象模型)，此文件定义了构建项目的一起配置信息。

##6、修改
默认的目录结构不完善，需要进行如下修改:

0. 添加src/main/java目录 java源码目录
0. 添加src/test/java目录  测试源码目录
0. 添加src/test/resources目录 测试资源目录
0. 添加target目录  输出(如果没有会自动创建)

~~~
C:\Users\Administrator\archetype\
│  pom.xml maven项目配置文件
│
├─src
│  ├─main
│  │  ├─java
│  │  ├─resources
│  │  └─webapp
│  │      └─WEB-INF
│  └─test
│      ├─java
│      └─resources
└─target
~~~	


#三、使用IDE

如果使用IDE集成maven，整个流程类似。此处不再赘述。

