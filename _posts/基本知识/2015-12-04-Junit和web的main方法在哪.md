---
layout: post
category: Java基础
title: junit和web的main方法在哪
date: 2015-12-04
---

标签 : java

<!-- more -->


## 引言

java程序跑起来是通过主类的main方法启动的。

开发的时候无论用eclipse还是idea这样的集成开发环境，只需要这个测试用例，在测试用例上右键一下，选着run junit，用例就跑起来了，我们并没有显示的写程序入口的main方法。

另外 javaweb 应用是怎么被tomcat启动起来的，应用中也没有main函数，tomcat作为应用容器将应用启起来，那么虚拟机跟tomcat又是什么关系呢？

## Junit

Junit 的启动顺序的过程如下：

>

    at org.junit.runner.JUnitCore.<init>(JUnitCore.java:35)
    at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:59)
    at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:211)
    at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:67)

可以看出是有JunitCore 开始的,JunitCore 的main方法如下所示

>

    public class JUnitCore {
        private final RunNotifier notifier = new RunNotifier();

        /**
         * Run the tests contained in the classes named in the <code>args</code>.
         * If all tests run successfully, exit with a status of 0. Otherwise exit with a status of 1.
         * Write feedback while tests are running and write
         * stack traces for all failed tests after the tests all complete.
         *
         * @param args names of classes in which to find tests to run
         */
        public static void main(String... args) {
            Result result = new JUnitCore().runMain(new RealSystem(), args);
            System.exit(result.wasSuccessful() ? 0 : 1);
        }




## tomcat

对于Web应用，是tomcat中按照Servlet等规范实现，我们的应用中写规范定义好的API逻辑，tomcat按请求去调用这些Servlet，从而启动Web应用。

tomcat的主类是BootStrap类，也是以此类的main方法作为入口启动的，如果要验证你可以看下tomcat的启动脚本。

>
set _EXECJAVA=%_RUNJAVA%
set MAINCLASS=org.apache.catalina.startup.Bootstrap
set ACTION=start


JDK包括了Java运行环境（Java Runtime Envirnment），一些Java工具和Java基础的类库(rt.jar)。

tomcat只是一种java应用服务器,所以它必须依赖JDK用来编译运行java程序。



