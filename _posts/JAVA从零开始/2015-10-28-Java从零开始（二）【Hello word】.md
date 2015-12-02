---
layout: post
title: Java从零开始（二）【Hello word】
category: Java基础
date: 2015-10-28
---

标签： Java


<!-- more -->

###（一）基本知识

Java文件需要经过编译后才可以运行
这个期间经过的步骤如下：

   编写Java源文件，后缀名是.java（就是我们要写的代码文件）

   经过  “javac” 编译， 后缀名是.class(这个就是编译后的文件，是自动生成的)


下来我们进行一个输出Hello word 的操作
###新建一个Hello.java的文件
需注意：  Java的文件名必须与public类型的类名相同，类名的第一个字母大写（不是一定要大写，但是得养成好习惯）。
里面写如下代码：


>
{% highlight  java %}
    public class Hello {
            // hello 就是类名  pubic 就是修饰符
	        public static void main(String [] args){
   		        System.out.print("hello word");
	        }
    }
{% endhighlight %}

###使用Javac进行编译，
注意： javac后面跟着的文件的名称


![java](/res/img/blogimg/javademo/20151113171441.png)


写好以后我们经过javac 编译后自动生成了.class文件


![javac](/res/img/blogimg/javademo/20151113171627.png)

看到.class 文件后就可以运行了


###使用java 进行运行

注意： java 后面跟着类的名称

运行后可以看见输出了“hello word”



![java](/res/img/blogimg/javademo/20151113172743.png)


这就是一个完整的从编写源代码，编译，运行的完整过程。












