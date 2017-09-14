#maven的安装
#一、概述
maven依赖于jdk，maven3.3+需要jdk6+。

maven不依赖平台，可以装在任何java支持的平台上。


#二、下载和安装
从官方网站`http://maven.apache.org/`的下载页面下载`zip`或`tar.gz`压缩包，然后解压到某个目录即可。

解压后目录结构如下:

~~~
apache-maven-<version>
├──bin   包含maven指令目录
├──boot maven启动时需要资源
├──conf maven配置目录，如全局配置setting.xml
├──lib maven运行时所需jar
│LICENSE
│NOTICE
│README.txt
~~~

#三、配置环境变量
##1、配置`JAVA_HOME `环境变量
指向jdk安装目录，记住jdk版本需6+。

##2、配置`M2_HOME`环境变量
配置`M2_HOME`指向maven安装目录，如:

~~~
M2_HOME=C:\Program Files\apache-maven-3.5.0
~~~

##3、添加到`PATH`环境变量

windows

~~~
PATH=...;%M2_HOME%/bin;
~~~

类unix系统

~~~
export PATH=/opt/apache-maven-3.5.0/bin:$PATH
~~~

#四、测试
命名行输入`mvn -v`，如果安装正确会看到**类似**如下输出:

~~~
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T04:57:37-07:00)
Maven home: /opt/apache-maven-3.3.3
Java version: 1.8.0_45, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.8.5", arch: "x86_64", family: "mac"
~~~




