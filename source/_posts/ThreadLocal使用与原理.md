---
title: ThreadLocal使用与原理(jdk8)
date: 2019-03-21 19:30:27
categories:
- Java进阶
---

## ThreadLocal是什么
<!--more-->
ThreadLocal是一个保存当前线程级别变量的类

## API
只有3个方法，都是对变量的操作方法
```
set(T)
get()
remove()
```



## 源码解读

### 源码
ThreadLocal 对外的方法：
```
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }
    
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }    
    
    public void remove() {
         ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
             m.remove(this);
     }
```

可以看到核心是对`ThreadLocalMap`操作


Thread类中保存了两个ThreadLocalMap
```
    /* ThreadLocal values pertaining to this thread. This map is maintained
     * by the ThreadLocal class. */
    ThreadLocal.ThreadLocalMap threadLocals = null;

    /*
     * InheritableThreadLocal values pertaining to this thread. This map is
     * maintained by the InheritableThreadLocal class.
     */
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
```

jdk对ThreadLocalMap解释
	>ThreadLocalMap is a customized hash map suitable only for
     maintaining thread local values. No operations are exported
     outside of the ThreadLocal class. The class is package private to
     allow declaration of fields in class Thread.  To help deal with
     very large and long-lived usages, the hash table entries use
     WeakReferences for keys. However, since reference queues are not
     used, stale entries are guaranteed to be removed only when
     the table starts running out of space.
源码中的注释说明这个类是一个专为线程变量定制的`hash map`，并且使用弱引用避免内存泄漏

### 为什么能线程隔离
存储的变量`ThreadLocalMap`是`Thread`的变量，每个线程对象都单独持有`ThreadLocalMap`，保证了线程隔离，而ThreadLocal类将自身作为key到`ThreadLocalMap`取线程变量。

## 拓展类
InheritableThreadLocal能够在起子线程时把变量传递，详细代码见`Thread`类



