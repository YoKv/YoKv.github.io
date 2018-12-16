---
title: redis数据结构-总结
date: 2018-12-16 17:30:27
categories:
- redis
---

## 总结
<!--more-->

### 内部数据结构

#### 字符串（sds）
基于c语言字符串，又有所不同，可以复用部分c语言字符串方法

#### 列表
双端链表

#### 字典(hash)
键值对结构，类似于java中hashmap

#### 集合（set）
不重复的值的组合

整数集合是集合键的底层实现之一，有序集合键的底层实现之一
##### 有序集合（sorted set）

#### 跳跃表

#### 压缩列表 

### 对外api对象
创建一个键值对的时候至少创建了两个对象：键对象，值对象。
```
SET msg 'msg'
TYPE msg
OBJECT ENCODING msg
```

#### 字符串对象 string

#### 列表对象 list

#### 哈希对象 hash

#### 集合对象 set
intset或者hashtable
#### 有序集合对象 zset
ziplist或者skiplist
