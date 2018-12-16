---
title: redis概述
date: 2017-11-09 17:30:27
categories:
- redis
---

##  介绍
Redis是一个开源（BSD许可）的内存数据结构存储，用作数据库，缓存和消息代理。它支持数据结构， strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries。 
<!--more-->

## 高连接问题
redis内部采用epoll技术，即多路复用IO。对于连接数非常高的时候，有着更好的处理性能，而且redis是单线程的避免了上下文切换。
Mysql每个连接都会生成一个对应的线程,MySQL把数据组织成树表，在磁盘和内存之间进行转换，并且进行大量的查找和排序，所有这些操作花费的时间都很长。那么 redis 为什么用非阻塞？主要是 redis 的数据目标是存储到内存，而且没有大密度查找排序这种计算，所以 redis 的计算很快，也就不存在 io 问题。连接数可以很高的本质在于内存处理速度快。


## 说明
redis读书笔记系列
[书本链接](https://book.douban.com/subject/25900156/)