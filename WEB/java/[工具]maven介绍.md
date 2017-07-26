# 安装
自行google/baidu

# 什么是Maven
Maven是基于项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建、报告和文档的软件项目管理工具。  
简单说,maven是一套强大的自动化构建工具，覆盖了项目的编译、测试、运行和打包的过程。maven提供了仓库的概念，统一地帮助我们管理第三方jar包。

# 项目结构
maven项目需要按照一定的项目结构进行维护，项目结构如下:  
```
src
  - main
    -java
      -package
  - test
    -java
      -package
  -resources
pom.xml
```

# 命令
maven的常用命令如下:  
mvn -v 查看maven版本  
mvn compile  编译项目  
mvn test  测试项目  
mvn package  打包项目  
mvn clean 删除target  
mvn intall 安装jar包到本地仓库中  
mvn archetype:generate 自动建立目录骨架  

# 构件和仓库
maven项目的jar包以及一些依赖和插件是通过仓库和构件的形式进行维护的，其中:  
构件由坐标唯一标识，pom测试文件，groupId、artifact和version可以组成项目的基本坐标  
仓库是来管理依赖的，本地仓库在~/.m2，远端仓库在http://search.maven.org.  

# pom.xml 配置
pom.xml是maven的配置文件，配置文件中各部分的主要含义如下:  
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<!-- 指定了当前pom的版本 -->
	<modelVersion>4.0.0</modelVersion>
	<!-- 公司网址反写 + 项目名 -->
	<groupId>com.sjming.girls</groupId>
	<!-- 项目名 + 模块名 -->
	<artifactId>girls</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<!-- 打包类型 jar/war/zip/pom -->
	<packaging>jar</packaging>
	<!-- 项目描述名 -->
	<name>girls</name>
    <!-- ulr 是项目地址 -->
	<!-- 项目描述 -->
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.4.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```

# 依赖
maven中关于构件的依赖是一个主要问题，重点主要是:  
依赖范围 `<scope>`  
依赖传递 排除依赖 `<exclusion>`  
依赖冲突 如果两个构件依赖不同版本的同一个构件，需要进行依赖冲突解决。原则是(1)短路优先原则(2)先声明者优先  
聚合与继承
