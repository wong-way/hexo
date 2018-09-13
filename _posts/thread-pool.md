---
title: thread-pool
date: 2018-08-02 10:14:03
categories: java
tags:
- thread pool
description: java线程池设计与使用
---
# 线程池总结

## 为什么要使用线程池

1. 创建/销毁线程伴随着系统开销，过于频繁的创建/销毁线程，会很大程度上影响处理效率 

   >例如：
   >
   >记创建线程消耗时间T1，执行任务消耗时间T2，销毁线程消耗时间T3
   >
   >如果T1+T3>T2，那么是不是说开启一个线程来执行这个任务太不划算了！
   >
   >正好，线程池缓存线程，可用已有的闲置线程来执行新任务，避免了T1+T3带来的系统开销

2. 线程并发数量过多，抢占系统资源从而导致阻塞

   > 我们知道线程能共享系统资源，如果同时执行的线程过多，就有可能导致系统资源不足而产生阻塞的情况
   >
   > 运用线程池能有效的控制线程最大并发数，避免以上的问题

3. 对线程进行一些简单的管理

   > 比如：延时执行、定时循环执行的策略等
   >
   > 运用线程池都能进行很好的实现

## 线程池`ThreadPoolExecutor`

### 构造函数

```java
//五个参数的构造函数
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue)

//六个参数的构造函数-1
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory)

//六个参数的构造函数-2
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          RejectedExecutionHandler handler)

//七个参数的构造函数
public ThreadPoolExecutor(int corePoolSize,
                          int maximumPoolSize,
                          long keepAliveTime,
                          TimeUnit unit,
                          BlockingQueue<Runnable> workQueue,
                          ThreadFactory threadFactory,
                          RejectedExecutionHandler handler)

```

* `int corePoolSize `：该线程池中核心线程的最大值

>  核心线程默认情况下会一直存活在线程池中，即使这个核心线程啥也不干(闲置状态)。
>
> 如果指定`ThreadPoolExecutor`的`allowCoreThreadTimeOut`这个属性为true，那么核心线程如果不干活(闲置状态)的话，超过一定时间(时长下面参数决定)，就会被销毁掉

* `int maximumPoolSize`：该线程池中线程的最大值

>线程总数 = 核心线程数 + 非核心线程数。 

* `long keepAliveTime `

> 该线程池中**非核心线程闲置超时时长**
>
> 一个非核心线程，如果不干活(闲置状态)的时长超过这个参数所设定的时长，就会被销毁掉如果设置`allowCoreThreadTimeOut = true`，则会作用于核心线程

* `TimeUnit unit`

> keepAliveTime的单位，TimeUnit是一个枚举类型，其包括：
>
> 1. NANOSECONDS ： 1微毫秒 = 1微秒 / 1000
> 2. MICROSECONDS ： 1微秒 = 1毫秒 / 1000
> 3. MILLISECONDS ： 1毫秒 = 1秒 /1000
> 4. SECONDS ： 秒
> 5. MINUTES ： 分
> 6. HOURS ： 小时
> 7. DAYS ： 天

* `BlockingQueue workQueue`

> 该线程池中的任务队列：维护着等待执行的Runnable对象
>
> 当所有的核心线程都在干活时，新添加的任务会被添加到这个队列中等待处理，如果队列满了，则新建非核心线程执行任务
>
> 常用的workQueue类型：
>
> 1. **SynchronousQueue：** 这个队列接收到任务的时候，会直接提交给线程处理，而不保留它，如果所有线程都在工作怎么办？那就新建一个线程来处理这个任务！所以为了保证不出现<线程数达到了maximumPoolSize而不能新建线程>的错误，使用这个类型队列的时候，maximumPoolSize一般指定成Integer.MAX_VALUE，即无限大
> 2. **LinkedBlockingQueue：** 这个队列接收到任务的时候，如果当前线程数小于核心线程数，则新建线程(核心线程)处理任务；如果当前线程数等于核心线程数，则进入队列等待。由于这个队列没有最大值限制，即所有超过核心线程数的任务都将被添加到队列中，这也就导致了maximumPoolSize的设定失效，因为总线程数永远不会超过corePoolSize
> 3. **ArrayBlockingQueue：** 可以限定队列的长度，接收到任务的时候，如果没有达到corePoolSize的值，则新建线程(核心线程)执行任务，如果达到了，则入队等候，如果队列已满，则新建线程(非核心线程)执行任务，又如果总线程数到了maximumPoolSize，并且队列也满了，则发生错误
> 4. **DelayQueue：** 队列内元素必须实现Delayed接口，这就意味着你传进去的任务必须先实现Delayed接口。这个队列接收到任务时，首先先入队，只有达到了指定的延时时间，才会执行任务

* `ThreadFactory threadFactory`

> 创建线程的方式，这是一个接口，你new他的时候需要实现他的`Thread newThread(Runnable r)`方法 

* `RejectedExecutionHandler handler `

> 抛出异常专用的，比如队列容量满了之后，就会由这个handler抛出异常，你不指定他也有个默认的 

新建一个线程池的时候，一般只用5个参数的构造函数。 

### 向ThreadPoolExecutor添加任务

通过`ThreadPoolExecutor.execute(Runnable command)`方法即可向线程池内添加一个任务 

### ThreadPoolExecutor的策略

上面介绍参数的时候其实已经说到了ThreadPoolExecutor执行的策略，这里给总结一下，当一个任务被添加进线程池时：

1. 线程数量未达到`corePoolSize`，则新建一个线程(核心线程)执行任务
2. 线程数量达到了`corePoolsize`，则将任务移入队列等待
3. 队列已满，新建线程(非核心线程)执行任务
4. 队列已满，总线程数又达到了`maximumPoolSize`，就会`RejectedExecutionHandler`抛出异常

## 常见四种线程池

如果你不想自己写一个线程池，那么你可以从下面看看有没有符合你要求，在大多数情况下都是足够使用的，Java通过`Executors`提供了四种线程池，这四种线程池都是直接或间接配置`ThreadPoolExecutor`的参数实现的，下面我都会贴出这四种线程池构造函数的源码

### `CachedThreadPool()`

可缓存线程池：

1. 线程数无限制
2. 有空闲线程则复用空闲线程，若无空闲线程则新建线程
3. 一定程序减少频繁创建/销毁线程，减少系统开销

创建方法：

`ExecutorService cachedThreadPool = Executors.newCachedThreadPool();`

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

### `FixedThreadPool()`

定长线程池：

1. 可控制线程最大并发数（同时执行的线程数）
2. 超出的线程会在队列中等待

创建方法：

```java
//nThreads => 最大线程数即maximumPoolSize
ExecutorService fixedThreadPool = Executors.newFixedThreadPool(int nThreads);
```

源码：

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

### `ScheduledThreadPool()`

定长线程池：

1. 支持定时及周期性任务执行。

创建方法：

```java
//nThreads => 最大线程数即maximumPoolSize
ExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(int corePoolSize);
```

源码：

```java
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize) {
    return new ScheduledThreadPoolExecutor(corePoolSize);
}

//ScheduledThreadPoolExecutor():
public ScheduledThreadPoolExecutor(int corePoolSize) {
    super(corePoolSize, Integer.MAX_VALUE,
          DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
          new DelayedWorkQueue());
}
```

### `SingleThreadExecutor()`

单线程化的线程池：

1. 有且仅有一个工作线程执行任务
2. 所有任务按照指定顺序执行，即遵循队列的入队出队规则

创建方法：

`ExecutorService singleThreadPool = Executors.newSingleThreadPool();`

源码：

```
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}
```

还有一个`Executors.newSingleThreadScheduledExecutor()`结合了3和4，就不介绍了，基本不用。