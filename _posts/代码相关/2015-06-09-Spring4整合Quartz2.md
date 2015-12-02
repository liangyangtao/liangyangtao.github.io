---
layout: post
title: Sping4.16整合Quartz2.2
category: 代码相关
date: 2015-06-09
---

标签： java  Spring Quartz

<!-- more -->

网络上好多代码都是Spring 3和 Quartz 1.8以下的配置
现在写的是Spring 4.1.6 和quartz 2.2.1 的


项目结构


maven

    <!-- Spring 4 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
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
			<artifactId>spring-context-support</artifactId>
			<version>4.1.6.RELEASE</version>
		</dependency>
		<!--quartz -->
		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz</artifactId>
			<version>2.2.1</version>
		</dependency>
		<dependency>
			<groupId>org.quartz-scheduler</groupId>
			<artifactId>quartz-jobs</artifactId>
			<version>2.2.1</version>
		</dependency>




Spring 配置文件代码：


	<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd">
	<!-- 注意写成你自己的 -->
	<context:component-scan base-package="com.unbank" />


	<!-- For times when you just need to invoke a method on a specific object -->
	<!-- <bean id="simpleJobDetail" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
		<property name="targetObject" ref="myBean" /> <property name="targetMethod"
		value="printMessage" /> </bean> -->


	<!-- For times when you need more complex processing, passing data to the
		scheduled job -->
	<bean name="complexJobDetail"
		class="org.springframework.scheduling.quartz.JobDetailFactoryBean">
			<!-- 注意写成你自己的 -->
		<property name="jobClass" value="com.unbank.duplicate.quartz.ScheduledJob" />
		<property name="jobDataMap">
			<map>
				<!-- 注意写成你自己的 -->
				<entry key="similarityBySimHash" value-ref="similarityBySimHash" />
			</map>
		</property>
		<property name="durability" value="true" />
	</bean>


	<!-- Run the job every 2 seconds with initial delay of 1 second -->
	<!-- <bean id="simpleTrigger" class="org.springframework.scheduling.quartz.SimpleTriggerFactoryBean">
		<property name="jobDetail" ref="simpleJobDetail" /> <property name="startDelay"
		value="1000" /> <property name="repeatInterval" value="2000" /> </bean> -->

	<!-- Run the job every 5 seconds only on Weekends -->
	<bean id="cronTrigger"
		class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
		<property name="jobDetail" ref="complexJobDetail" />
		<property name="cronExpression" value="01 02 01 * * ?" />
	</bean>


	<!-- Scheduler factory bean to glue together jobDetails and triggers to
		Configure Quartz Scheduler -->
	<bean class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
		<property name="jobDetails">
			<list>
				<!-- <ref bean="simpleJobDetail" /> -->
				<ref bean="complexJobDetail" />
			</list>
		</property>
		<property name="triggers">
			<list>
				<!-- <ref bean="simpleTrigger" /> -->
				<ref bean="cronTrigger" />
			</list>
		</property>
	</bean>
</beans>



 业务调度类


    package com.unbank.duplicate.quartz;
    import org.apache.log4j.Logger;
    import org.quartz.JobExecutionContext;
    import org.quartz.JobExecutionException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.scheduling.quartz.QuartzJobBean;
    import com.unbank.duplicate.SimilarityBySimHash;
    public class ScheduledJob extends QuartzJobBean {
    	public static Logger logger = Logger.getLogger(ScheduledJob.class);
    	// 这里的类写自己的
    	@Autowired
    	private SimilarityBySimHash similarityBySimHash;
        // 定时启动后程序的入口
    	@Override
    	protected void executeInternal(JobExecutionContext arg0)
    			throws JobExecutionException {
    		similarityBySimHash.load();
    	}
        public void setSimilarityBySimHash(SimilarityBySimHash similarityBySimHash) {
    		this.similarityBySimHash = similarityBySimHash;
    	}
    }




   业务调度类结束


项目地址：
https://github.com/liangyangtao
下面的
ubk_duplicate

























