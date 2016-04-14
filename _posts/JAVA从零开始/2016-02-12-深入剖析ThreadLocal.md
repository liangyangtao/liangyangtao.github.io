---
layout: post
title: 深入剖析ThreadLocal
category: Java基础
date: 2016-02-12
---


标签： Java 多线程



<!-- more -->

## ThreadLocal

ThreadLocal，很多地方叫做线程本地变量，也有些地方叫做线程本地存储，其实意思差不多。

可能很多朋友都知道ThreadLocal为变量在每个线程中都创建了一个副本，那么每个线程可以访问自己内部的副本变量。

当然ThreadLocal并不能替代同步机制，两者面向的问题领域不同。

同步机制是为了同步多个线程对相同资源的并发访问，是为了多个线程之间进行通信的有效方式；

而ThreadLocal是隔离多个线程的数据共享，从根本上就不在多个线程之间共享资源（变量），这样当然不需要对多个线程进行同步了。

所以，如果你需要进行多个线程之间进行通信，则使用同步机制；如果需要隔离多个线程之间的共享冲突，可以使用ThreadLocal，这将极大地简化我们的程序，使程序更加易读、简洁。

ThreadLocal类为各线程提供了存放局部变量的场所。

ThreadLocal 不是用来解决共享对象的多线程访问问题的，一般情况下，通过ThreadLocal.set() 到线程中的对象是该线程自己使用的对象，其他线程是不需要访问的，也访问不到的。各个线程中访问的是不同的对象。

另外，说ThreadLocal使得各线程能够保持各自独立的一个对象，并不是通过ThreadLocal.set()来实现的，而是通过每个线程中的new 对象 的操作来创建的对象，每个线程创建一个，不是什么对象的拷贝或副本。


###  ThreadLocal类提供的几个方法：

>
    private final int threadLocalHashCode = nextHashCode();
    private static AtomicInteger nextHashCode = new AtomicInteger();
    private static final int HASH_INCREMENT = 0x61c88647;

    public T get() { }
    public void set(T value) { }
    public void remove() { }
    protected T initialValue() { }


get()方法是用来获取ThreadLocal在当前线程中保存的变量副本，set()用来设置当前线程中变量的副本，remove()用来移除当前线程中变量的副本，initialValue()是一个protected方法，一般是用来在使用时进行重写的，它是一个延迟加载方法，下面会详细说明。

首先我们来看一下ThreadLocal类是如何为每个线程创建一个变量的副本的。

先看下get方法的实现：

>
{% highlight java %}
      /**
         * Returns the value in the current thread's copy of this
         * thread-local variable.  If the variable has no value for the
         * current thread, it is first initialized to the value returned
         * by an invocation of the {@link #initialValue} method.
         *
         * @return the current thread's value of this thread-local
         */
        public T get() {
            Thread t = Thread.currentThread();
            ThreadLocalMap map = getMap(t);
            if (map != null) {
                ThreadLocalMap.Entry e = map.getEntry(this);
                if (e != null)
                    return (T)e.value;
            }
            return setInitialValue();
        }

 {% endhighlight %}

第一句是取得当前线程，然后通过getMap(t)方法获取到一个map，map的类型为ThreadLocalMap。然后接着下面获取到<key,value>键值对，注意这里获取键值对传进去的是  this，而不是当前线程t。

如果获取成功，则返回value值。

如果map为空，则调用setInitialValue方法返回value。

我们上面的每一句来仔细分析：

首先看一下getMap方法中做了什么：

>
{% highlight java %}

       ThreadLocalMap getMap(Thread t) {
                     return t.threadLocals;
                 }

{% endhighlight %}
　　

可能大家没有想到的是，在getMap中，是调用当期线程t，返回当前线程t中的一个成员变量threadLocals。

那么我们继续取Thread类中取看一下成员变量threadLocals是什么：

>
{% highlight java %}
     ThreadLocal.ThreadLocalMap threadLocals = null;

{% endhighlight %}

实际上就是一个ThreadLocalMap，这个类型是ThreadLocal类的一个内部类，我们继续取看ThreadLocalMap的实现：

>
{% highlight java %}

      static class ThreadLocalMap {

            /**
             * The entries in this hash map extend WeakReference, using
             * its main ref field as the key (which is always a
             * ThreadLocal object).  Note that null keys (i.e. entry.get()
             * == null) mean that the key is no longer referenced, so the
             * entry can be expunged from table.  Such entries are referred to
             * as "stale entries" in the code that follows.
             */
            static class Entry extends WeakReference<ThreadLocal> {
                /** The value associated with this ThreadLocal. */
                Object value;

                Entry(ThreadLocal k, Object v) {
                    super(k);
                    value = v;
                }
            }

{% endhighlight %}


可以看到ThreadLocalMap的Entry继承了WeakReference，并且使用ThreadLocal作为键值。

然后再继续看setInitialValue方法的具体实现：

>
{% highlight java %}
     /**
         * Variant of set() to establish initialValue. Used instead
         * of set() in case user has overridden the set() method.
         *
         * @return the initial value
         */
        private T setInitialValue() {
            T value = initialValue();
            Thread t = Thread.currentThread();
            ThreadLocalMap map = getMap(t);
            if (map != null)
                map.set(this, value);
            else
                createMap(t, value);
            return value;
        }

{% endhighlight %}


很容易了解，就是如果map不为空，就设置键值对，为空，再创建Map，看一下createMap的实现：

>

        void createMap(Thread t, T firstValue) {
            t.threadLocals = new ThreadLocalMap(this, firstValue);
        }



至此，可能大部分朋友已经明白了ThreadLocal是如何为每个线程创建变量的副本的：

首先，在每个线程Thread内部有一个ThreadLocal.ThreadLocalMap类型的成员变量threadLocals，这个threadLocals就是用来存储实际的变量副本的，键值为当前ThreadLocal变量，value为变量副本（即T类型的变量）。

初始时，在Thread里面，threadLocals为空，当通过ThreadLocal变量调用get()方法或者set()方法，就会对Thread类中的threadLocals进行初始化，并且以当前ThreadLocal变量为键值，以ThreadLocal要保存的副本变量为value，存到threadLocals。

然后在当前线程里面，如果要使用副本变量，就可以通过get方法在threadLocals里面查找。


## 参考

http://www.cnblogs.com/dolphin0520/p/3920407.html