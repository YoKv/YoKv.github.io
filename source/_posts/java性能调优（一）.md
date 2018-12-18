---
title: java性能调优（一）.md
date: 2018-12-18 17:30:27
categories:
- Java进阶
---

# 介绍
<!--more-->
读书笔记
[📕链接](https://baike.baidu.com/item/Java%E7%A8%8B%E5%BA%8F%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E2%80%94%E2%80%94%E8%AE%A9%E4%BD%A0%E7%9A%84Java%E7%A8%8B%E5%BA%8F%E6%9B%B4%E5%BF%AB%E3%80%81%E6%9B%B4%E7%A8%B3%E5%AE%9A)

## 性能体现方面
* 执行速度
* 内存分配
* 启动时间
* 负载承受能力

## 指标
* 执行时间
* cpu时间
* 内存分配
* 磁盘吞吐量
* 网络吞吐量
* 响应时间

系统性能优化是典型的木桶原理，先优化性能瓶颈

## 优化层次
* 设计调优
* 代码调优
* jvm调优
* 数据库调优
* 操作系统调优


# 设计优化
善用设计模式

## 单例模式
单例模式的对象避免了每次使用都创建对象。
简单的单例实现不能惰性加载，加上同步锁后又有性能损失，可以使用内部类维护单例的实例。单例并不一定总是只有一个对象，比如使用反射非法操作时，或者使用序列化和反序列化合法生成对象（可以实现深克隆）

## 代理模式
系统启动时，将消耗资源多方法用代理模式分离，加快启动时间。用代理模式实现延时加载。

### 代理模式几种实现

**接口**
```
public interface Foo {
    String bar();
}
```
**慢实现**
```
public class FooImpl implements Foo {
    @Override
    public String bar() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "bar";
    }
}

```

**常规代理**
```
public class FooProxy implements Foo {
    private FooImpl foo;

    @Override
    public String bar() {
        if (null == foo) {
            foo = new FooImpl();
        }
        return foo.bar();
    }
}
```

**动态代理**
jdk的动态代理

TODO


几种代理比较





