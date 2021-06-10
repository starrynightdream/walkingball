---
title: 居然是JAVA的maven
date: 2021-06-10 19:51:05
tags:
- 技术
	+ java
	+ maven
thumbnail: /walkingball/images/kon-java-=w=.jpg
---

# maven的出现让我觉得这个框架变得简洁了
说实话，之前不觉得SSM简洁，原因大抵是那导包的地狱。但是maven的学习让我明白，自己还是嫩。

## 问题集合部分

### Select an Archetype为空
重新运行下面的命令，无需创建，估计是什么没下好。

	mvn archetype:generate

注意可能要重启，我的情况没有重启。

### 编码相关
你应该在这里修改你的源文件编码
```xml
<project>
  ...
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
  ...
</project>
```

### servlet-api.jar 包 / javax.servlet.http 不存在等
注意scope
```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
```

### annot invoke "org.apache.ibatis.parsing.XNode.getStringAttribute(String)" because "context" is null
因为曾把mybatis-config.xml位置放错。因此导致其maven target文件夹未更新。

clean就好。项目右键 -> run as -> maven clea

###  "context:component-scan"  "context" 未绑定
注意是否有指定context

	xmlns:context="http://www.springframework.org/schema/context" 

### java.lang.ClassNotFoundException javax.xml.bind.ValidationException
[待细看](https://blog.csdn.net/sihai12345/article/details/80744012)

### tomcat9 热运行
使用
```xml
	<server>
   		<id>tomcat</id>
   		<username>tomcat</username>
   		<password>tomcat</password>
	</server>
```
待补充


## 其它记录的解决方案

### maven网站查找
[这里](https://mvnrepository.com/)

### maven依赖导入错误
未尝试，仅记录

1. 切换阿里源和默认源
2. 删除.lastUpdated 文件

## classpath
classpath对应的路径其实就是项目中的src文件中。
但在maven里貌似是src/main/resources
