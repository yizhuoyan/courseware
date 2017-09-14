#maven配置

#一、概述
maven的配置分为两个级别，全局和用户。

全局的配置位于maven安装目录下的`config/setting.xml`

用户的配置位于用户工作目录下`.m2/setting.xml`

用户级别优先级高于全局配置。

两者配置文件格式一样。

#二、配置
全局配置文件`setting.xml`位于`M2_HOME`下的config目录中。

主要有如下配置:

##1、本地库路径
默认是当前用户工作目录下的/.m2/repository目录下。
如windows中的Administrator用户，则默认本地库路径为:

`C:\Users\Administrator\.m2\repository`

可通过`<localRepository>`重新指定。

~~~xml
 <localRepository>/path/to/local/repo</localRepository>
~~~
##2、是否离线使用maven
离线使用maven的含义是在进行相关构建的过程是否回去连接网络，默认会。

~~~xml
<offline>false</offline>
~~~

##3、镜像
用于配置远程库的地址。如maven官方的中央库(由于某些你懂得的原因)下载速度巨慢，一般把中央库的地址设置为国内镜像。

我们这里采用阿里云。

~~~xml
<mirrors>
....
    <mirror>
        <id>nexus-aliyun</id>
		<!--镜像中英库-->
        <mirrorOf>central</mirrorOf>   
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror> 
</mirrors>
~~~

#三、第一次运行
在命令行输入命令`mvn help:system`，此命令的作用是显示当前maven工作的环境变量和属性。我们用此命令来测试我们的配置是否生效。

~~~
省略一大波下载输出。。。
===============================================================================
========================= Platform Properties Details =========================
===============================================================================

===============================================================================
System Properties
===============================================================================

java.runtime.name=Java(TM) SE Runtime Environment
sun.boot.library.path=C:\Program Files\Java\jdk1.8.0_121\jre\bin
java.vm.version=25.121-b13
.....(省略其他类似输出)

===============================================================================
Environment Variables
===============================================================================

CLASSWORLDS_JAR="C:\opt\apache-maven\apache-maven-3.3.9\boot\plexus-classworlds-
2.5.2.jar"
.....(省略其他类似输出)

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.101 s
[INFO] Finished at: <完成时间>
[INFO] Final Memory: 9M/150M
[INFO] ------------------------------------------------------------------------
~~~



