---
title: jvm特性（6）并发
date: 2020-03-12 17:30:29
categories:
- Java进阶
---


# 内存
<!--more-->
主内存，工作内存

### 内存操作
* lock
* unlock
* read
* load
* use
* assign
* store
* write


### volatile
不能用于 i++这种非原子操作，其中包含了3个操作
防止指令重排序

# 线程
* 内核线程实现
* 用户线程实现
* 用户线程实现加轻量级进程混合实现

### 线程调度
* 协同式线程调度
* 抢占式线程调度

### 状态转换
* 新建
* 运行
* 无限期等待
    > wait(); join(); LockSupport.park();
* 限期等待
    > sleep(1); wait(1); join(1); LockSupport.parkNanos(); LockSupport.parkUntil();
* 阻塞
    > 等待获取排他锁
* 结束


# 线程安全与锁优化