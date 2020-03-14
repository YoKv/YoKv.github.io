---
title: jvm特性（7）并发
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

操作线程共享数据程度：
* 不可变
* 绝对线程安全
* 相对线程安全
* 线程兼容
* 线程对立


### 线程安全实现

#### 互斥同步
synchronized
ReentrantLock

#### 非阻塞同步
AtomicInteger
乐观执行，出现冲突补偿
CAS
#### 无同步方案
* 可重入代码
* 线程本地存储


### 锁优化
* 自旋锁与自适应自旋
* 锁消除
* 锁粗化
* 轻量级锁
* 偏向锁








