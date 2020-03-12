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
