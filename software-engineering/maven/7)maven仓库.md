#maven仓库

#一、概述
maven的仓库分为两大类，本地仓库和远程仓库，当需要某个artifact时，会首先去本地仓库中查找，如果没有，则去远程仓库下载，放入本地仓库后进行使用。

- 本地仓库

本地仓库可通过配置文件`setting.xml`中的`<localRepository/>`标签指定。默认是用户目录下的`.m2/repository目录`。

- 远程仓库
远程仓库可分为三类:

	1)中央仓库

	默认远程的中央仓库配置为:

	~~~xml
<repository>
	<id>central</id>
	<name>Central Repository</name>
	<url>https://repo.maven.apache.org/maven2</url>
	<layout>default</layout>
	<snapshots>
		<enabled>false</enabled>
	</snapshots>
</repository>
~~~

	2)局域网仓库(私服):

	每个公司可以在局域网中搭建一个maven服务器，用于整个公司所有项目。

	3)第三方公共库:

	如我们使用的aliyun

#二、安装库到本地项目
我们可以把我们的项目安装到本地的仓库，用于项目模块间的依赖。

使用如下命令即可。

~~~
mvn  clean install
~~~
默认执行clean生命周期的pre-clean和clean两个阶段，然后执行default生命周期中install以前的阶段和install阶段。

intall阶段的任务是把打包好的文件拷贝到本地库对应目录中。此时依赖于此模块的写法和其他依赖一致。

#三、配置远程仓库

可以在当前项目下的`pom.xml`中配置其他远程库。maven在下载依赖时会依次请求这些远程库和中央库。直到完毕为止。

~~~xml
<repositories>
	<repository>
		<id>sonatype-nexus-snapshots</id>
		<name>Sonatype Nexus Snapshots</name>
		<url>http://oss.sonatype.org/content/repositories/snapshots</url>
		<releases>
			<enabled>false</enabled>
		</releases>
		<snapshots>
				<enabled>true</enabled>
		</snapshots>
	</repository>
	<!--开源中国的库-->
	<repository>
		<id>oschina</id>
		<name>local private nexus</name>
		<url>http://maven.oschina.net/content/groups/public/</url>
		<releases>
			<enabled>true</enabled>
		</releases>
		<snapshots>
			<enabled>false</enabled>
		</snapshots>
	</repository>
</repositories>
	<!--spring提供的库-->
 <repository>
        <id>io.spring.repo.maven.release</id>
        <url>http://repo.spring.io/release/</url>
        <snapshots><enabled>false</enabled></snapshots>
</repository>
~~~

#四、镜像
如果A仓库可以提供B仓库的所有内容，则成为A是B的一个镜像了。由于maven中央库在国外，一般国内开发都需要配置镜像。

配置针对中央库的镜像:**apatch-sonatype**

~~~ xml
<mirror>
    <id>sonatype</id>
    <name>sonatype Central</name>
    <url>http://repository.sonatype.org/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
~~~

**ibiblio**-亲测可用

~~~
<mirror>
    <id>ibiblio</id>
    <mirrorOf>central</mirrorOf>
    <name>Human Readable Name for this Mirror.</name>
    <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>
</mirror>
~~~
配置针对所有库的镜像:注意mirrorOf的使用

~~~
<mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>*</mirrorOf>
</mirror>
~~~

#五、仓库搜索
如何确定仓库中依赖的坐标呢?可以从如下网站上查询:

- https://repository.sonatype.org/index.html
- https://search.maven.org
- http://www.mvnrepository.com/


