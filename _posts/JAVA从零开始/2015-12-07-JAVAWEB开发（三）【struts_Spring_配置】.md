---
layout: post
title:  JAVAWEB开发（三）【struts_Spring_配置】
category: Java基础
date: 2015-12-07
---

标签： Java


<!-- more -->



##前言

 上节我们用maven 搭建一个WEb项目，这节我们介绍struts Spring 的应用，如果想补充基本知识请自己百度。

## 一部署测试Web项目到Tomcat



  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207150804.png)

   ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207150956.png)

   ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207151038.png)


   ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207151053.png)

   ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207151447.png)

   ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207151128.png)



   等待服务启动以后

  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207151506.png)



  在浏览器输入

  http://localhost:8080/apartment_MS/


  如果看到HelloWord 说明部署到Tomcat成功

  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207152039.png)



## 添加配置文件

### (一) 在pom.xml 中配置struts Spring 等 依赖的包

一般配置文件如下，（<!-- XXX --> 表示注释 ）

直接复制下面文件即可，点击保存后，需联网，这时候会卡一段时间，原因是maven 在下载依赖的包，下载完成后会在你的本地maven 库中。
如果pom.xml出现 “！”错误，说明你的包没有下完。
解决方法就是删除掉本地maven 库中没有下载完成的那个包的文件夹（本地maven库见下图），在重新下载就行了。由于“长城墙”的原因，可能下载不顺利。多试几次就行了。
  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207153250.png)


  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207152926.png)

>
{% highlight java %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.apartment</groupId>
	<artifactId>apartment_MS</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>apartment_MS Maven Webapp</name>
	<url>http://maven.apache.org</url>
	<dependencies>
		<!-- dom4j -->
		<dependency>
			<groupId>dom4j</groupId>
			<artifactId>dom4j</artifactId>
			<version>1.6.1</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.10</version>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.8.1</version>
		</dependency>

		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.4</version>
		</dependency>
		<!-- json -->
		<dependency>
			<groupId>net.sf.json-lib</groupId>
			<artifactId>json-lib</artifactId>
			<version>2.4</version>
			<classifier>jdk15</classifier>
		</dependency>


		<!-- sftp -->
		<dependency>
			<groupId>com.jcraft</groupId>
			<artifactId>jsch</artifactId>
			<version>0.1.53</version>
		</dependency>
		<!-- htttpclient -->
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<version>4.4.1</version>
		</dependency>
		<!-- jdbc -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.1</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-core</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.2.7</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.18</version>
		</dependency>
		<dependency>
			<groupId>com.mchange</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.5</version>
		</dependency>
		<!-- struts -->
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-core</artifactId>
			<version>2.3.24</version>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-convention-plugin</artifactId>
			<version>2.3.24</version>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-json-plugin</artifactId>
			<version>2.3.24</version>
		</dependency>
		<dependency>
			<groupId>org.apache.struts</groupId>
			<artifactId>struts2-spring-plugin</artifactId>
			<version>2.3.24</version>
			<exclusions>
				<exclusion>
					<artifactId>spring-aop</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
				<exclusion>
					<artifactId>spring-context</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
				<exclusion>
					<artifactId>spring-beans</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
				<exclusion>
					<artifactId>spring-web</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
				<exclusion>
					<artifactId>spring-core</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
			</exclusions>
		</dependency>

		<!-- Spring 4 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>1.8.5</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.8.5</version>
		</dependency>
	</dependencies>
	<build>
		<finalName>apartment_MS</finalName>
	</build>
 </project>


{% endhighlight %}

## 添加log4j.properties 日志配置文件
>
{% highlight java %}
log4j.rootLogger=INFO,A1,A2,DEBUG
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.Threshold=INFO
log4j.appender.A2=org.apache.log4j.DailyRollingFileAppender
log4j.appender.A2.Threshold=INFO
log4j.appender.A2.File=./log/dailyLog/ENDailyRollingLog.log
log4j.appender.A2.DatePattern='.'yyyy-MM-dd'.log'

log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} <# %l> %-5p [%r]:%n%m%n
log4j.appender.A2.layout=org.apache.log4j.PatternLayout
log4j.appender.A2.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} <# %l> %-5p [%r]:%n%m%n

log4j.logger.com.ibatis=DEBUG
log4j.logger.com.ibatis.common.jdbc.SimpleDataSource=DEBUG
log4j.logger.com.ibatis.common.jdbc.ScriptRunner=DEBUG
log4j.logger.com.ibatis.sqlmap.engine.impl.SqlMapClientDelegate=DEBUG
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql.Connection=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG

{% endhighlight %}


### 添加struts 配置文件struts.xml
>
{% highlight java %}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 2.1//EN" "http://struts.apache.org/dtds/struts-2.1.dtd">
<struts>
	<constant name="struts.i18n.reload" value="false" />
	<constant name="struts.devMode" value="false" />
	<constant name="struts.configuration.xml.reload" value="false" />
	<constant name="struts.custom.i18n.resources" value="globalMessages" />
	<constant name="struts.action.extension" value="action,," />

	<constant name="struts.i18n.encoding" value="UTF-8" />
	<constant name="struts.enable.DynamicMethodInvocation" value="false" />

	<constant name="struts.ui.theme" value="simple" />
	<constant name="struts.ui.templateDir" value="template" />
	<constant name="struts.multipart.maxSize" value="100000000" />
	<constant name="struts.enable.SlashesInActionNames" value="true" />
	<constant name="struts.devMode" value="false" />

	<!-- 临时文件目录 -->
	<constant name="struts.multipart.saveDir" value="tmp"></constant>
	<package name="default" namespace="/" extends="struts-default">

		<result-types>
			<result-type name="json" class="org.apache.struts2.json.JSONResult" />
		</result-types>

		<interceptors>
			<!-- 定义session控制拦截器 -->
			<interceptor-stack name="mydefault">
				<interceptor-ref name="defaultStack" />
			</interceptor-stack>
		</interceptors>

		<!-- 定义默认拦截器 -->
		<default-interceptor-ref name="mydefault" />

		<global-results>
			<!-- 逻辑名为sessionTimeout的结果，映射到/sessionTimeout.jsp页面 -->
			<result name="sessionTimeout">/sessionTimeout.jsp</result>
			<result name="error">/errorPage.jsp</result>
		</global-results>

		<global-exception-mappings>
			<exception-mapping exception="java.lang.Exception"
				result="error" />
		</global-exception-mappings>

	</package>



</struts>



{% endhighlight %}

## 添加Spring 配置文件

>
{% highlight java %}
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/context/spring-aop-3.0.xsd">

	<context:component-scan base-package="com.unbank" />

	<!-- 获取JDBC配置文件 -->
	<bean
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="locations">
			<value>classpath:jdbc.properties</value>
		</property>
	</bean>
	<!-- MySql数据源的配置，采用DBCP连接池 -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<property name="driverClass">
			<value>${database.driverClassName}</value>
		</property>
		<property name="jdbcUrl">
			<value>${database.url}</value>
		</property>
		<property name="user">
			<value>${database.username}</value>
		</property>
		<property name="password">
			<value>${database.password}</value>
		</property>
		<property name="minPoolSize">
			<value>${database.minPoolSize}</value>
		</property>
		<property name="maxPoolSize">
			<value>${database.maxPoolSize}</value>
		</property>
		<property name="initialPoolSize">
			<value>${database.initialPoolSize}</value>
		</property>
		<property name="maxIdleTime">
			<value>${database.maxIdleTime}</value>
		</property>
		<property name="acquireIncrement">
			<value>${database.acquireIncrement}</value>
		</property>
		<property name="acquireRetryAttempts">
			<value>${database.acquireRetryAttempts}</value>
		</property>
		<property name="acquireRetryDelay">
			<value>${database.acquireRetryDelay}</value>
		</property>
		<property name="testConnectionOnCheckin">
			<value>${database.testConnectionOnCheckin}</value>
		</property>
		<property name="automaticTestTable">
			<value>${database.automaticTestTable}</value>
		</property>
		<property name="idleConnectionTestPeriod">
			<value>${database.idleConnectionTestPeriod}</value>
		</property>
		<property name="checkoutTimeout">
			<value>${database.checkoutTimeout}</value>
		</property>
	</bean>


	<!-- 事务 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>


	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation" value="classpath:mybatis.xml" />
	</bean>

	<!-- scan for mappers and let them be autowired -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- Mapper接口所在包名，Spring会自动查找其下的Mapper -->
		<property name="basePackage" value="com.unbank.mybatis.mapper" />
		<!-- <property name="sqlSessionFactory" ref="sqlSessionFactory"></property> -->
	</bean>


</beans>

{% endhighlight %}

## 添加jdbc.properties

>
{% highlight java %}

database.driverClassName =com.mysql.jdbc.Driver
database.url =jdbc:mysql://localhost:3306/apartment_ms?autoReconnect=true&amp;characterEncoding=UTF-8
database.username =spider
database.password =spider
database.hostname=localhost
database.name = qiankong
database.backup=\\backup\\database\\
database.minPoolSize = 1
database.maxPoolSize = 200
database.initialPoolSize = 20
database.maxIdleTime = 25000
database.acquireIncrement = 1
database.acquireRetryAttempts = 30
database.acquireRetryDelay = 1000
database.testConnectionOnCheckin = true
database.automaticTestTable = ttttt
database.idleConnectionTestPeriod = 18000
database.checkoutTimeout=5000
database.preferredTestQuery='SELECT 1'
database.testConnectionOnCheckout=true


{% endhighlight %}

## 添加mybatis.xml

>
{% highlight java %}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

	<mappers>
		<!--userMapper.xml装载进来 同等于把“dao”的实现装载进来
		<mapper resource="com/unbank/mybatis/mapper/ArticleInfoMapper.xml" />
		-->

	</mappers>
</configuration>

{% endhighlight %}


## 修改Web.xml,添加Struts ，Spring 过滤器

  ![安装MyEclipse插件](/res/img/blogimg/20151207/20151207154043.png)


>
{% highlight java %}

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
	<display-name></display-name>
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>

	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>


	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter
		</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext*.xml</param-value>
	</context-param>

	<filter>
		<filter-name>struts2</filter-name>
		<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>*.action</url-pattern>
	</filter-mapping>
</web-app>

{% endhighlight %}















