---
title: kotlin coroutines学习摘要
tags:
  - kotlin
  - language
  - kotlin coroutine
categories:
  - kotlin
abbrlink: ec789f13
date: 2018-07-16 23:21:51
---
kotlin coroutines学习摘要。协程的定义及优缺点。kotlin coroutine关键字和概念的初步理解。在安卓开发下的应用场景及简单应用。
入门级别。
命令行工具
<!-- more -->
## kotlin coroutines
kotlin版的协程，比线程更轻量的异步任务操作实现。语言级别模拟实现“多线程”效果。

## coroutine vs thread
线程属于系统级概念，由系统调度执行  
协程属于语言级概念，由应用自己管理调度/挂起/执行协程。

## 优点
1. 轻量性能好  
协程的挂起（suspend）对应线程的阻塞（block），协程上下文切换对应线程的上下文切换
利用挂起（suspend）当前上下文（协程上下文切换，轻量级）来代替阻塞线程（线程上下文切换，重量级）  
2. 资源利用率高  
可以复用被delay的线程资源。
3. 可以使用同步的编写方式编写异步回调代码

## 缺点
某些场景下协程反复使用同一格线程，多核处理器优势没有发挥

## 可以应用到生产环境了吗
可以 [refer this](https://stackoverflow.com/questions/46240236/can-experimental-kotlin-coroutines-be-used-in-production/46240340)  
The current version of kotlinx.coroutines is designed for production use. It is pretty well covered with tests, lots of things are already optimized, all the changes are made considering the issues of backwards compatibility with previously compiled code.

It certainly does serve as a test-bed for various coroutine-based things, so there are some parts that are clearly marked as "work in progress" or "unstable" in the documentation of the corresponding funs/classes. However, by default, all the public APIs in kotlinx.coroutines are considered to be stable.



## 应用场景：
1. 轻量异步任务，就没必要开线程处理了  
   客户端：单次网络请求，db/sp操作，文件读写，图片处理
2. 高负荷网络IO、文件IO、CPU/GPU密集型任务，可以极大减少了线程数量，减少cpu占用  
   服务端：高并发网络请求处理


## 安卓应用场景
1. rxjava异步+回调可以替换为协程异步+结果同步处理，[解决callback hell问题](https://medium.com/@andrea.bresolin/playing-with-kotlin-in-android-coroutines-and-how-to-get-rid-of-the-callback-hell-a96e817c108b)
2. retrofit+coroutines 
3. room + corountines


## coroutine关键字
**Coroutine context**： 协程的运行上下文，每个协程都运行在一个CoroutineContext中。上下文是一个比较泛泛的概念，可以是一个环境，可以是一个容器，提供各种运行时资源和状态。这里Coroutine context包含两个概念：CoroutineDispatcher和job（都是CoroutineContext的子类...），
CoroutineDispatcher提供job执行时使用的线程，job顾名思义定义了一个具体可执行的任务。这两个加起来就构成了cotoutine上下文  

原文：
Coroutines always execute in some context which is represented by the value of CoroutineContext type, defined in the Kotlin standard library.

The coroutine context is a set of various elements. The main elements are the Job of the coroutine, which we've seen before, and its dispatcher, which is covered in this section.

**launch**：开一个协程，返回一个job引用 Launches new coroutine without blocking current thread and returns a reference to the coroutine as a Job. The coroutine is cancelled when the resulting job is cancelled.

**async**：开一个协程，返回一个deffered引用（带future结果的特殊job）async is just like launch. It starts a separate coroutine which is a light-weight thread that works concurrently with all the other coroutines. The difference is that launch returns a Job and does not carry any resulting value, while async returns a Deferred.

**UI**：安卓的主线程上下文  
```
val UI = HandlerContext(Handler(Looper.getMainLooper()), "UI")
```

**Deferred**：轻量级、非阻塞的 future，可以通过await()来获得结果 a light-weight non-blocking future that represents a promise to provide a result later. You can use .await() on a deferred value to get its eventual result, but Deferred is also a Job, so you can cancel it if needed.

**await()**：启动一个新的协程，挂起当前coroutine直到新协程执行完并返回值

**suspend function**: 只能运行在coroutine中，在新的coroutine中执行时挂起当前coroutine
- [suspend vs blocking](https://medium.com/@elye.project/understanding-suspend-function-of-coroutines-de26b070c5ed)

## 简单异步回调用协程同步实现
```
//创建新的UI协程
launch(UI) {
//1 创建新的thread协程
val deferred = async{
//2-2 thread协程 do something 
} 
//2-1 挂起当前协程，执行async开启的新协程
val result = deferred.await() 
//3 回到ui协程 do something with the result
}
```


## 参考：
[线程上下文切换](https://www.cnblogs.com/xrq730/p/5186609.html)
即使是单核CPU也支持多线程执行代码，CPU通过给每个线程分配CPU时间片来实现这个机制。时间片是CPU分配给各个线程的时间，因为时间片非常短，所以CPU通过不停地切换线程执行，让我们感觉多个线程时同时执行的，时间片一般是几十毫秒（ms）。
CPU通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再次加载这个任务的状态，从任务保存到再加载的过程就是一次上下文切换。
