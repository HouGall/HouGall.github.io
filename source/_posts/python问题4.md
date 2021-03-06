---
title: python问题4
data: 2020-8-15 18:50:18
tags:
 - java
 - 求职
categories:
 - 求职
mathjax: true
---
# 垃圾回收机制
>python中的垃圾回收机制和java类似，采用的机制是以引用计数为主，标记-清除和分代收集为辅的策略。

## 引用计数
>python中所有的东西都是对象，他们的核心既是一个结构体，即pyobject，其结构体是:

``` c++
typedef struct_object {
    int ob_refcnt;
    struct_typeobject *ob_type;
} PyObject;
```
>pyobject是每个对象都有的内容，其中ob_refcnt即使引用计数，每当对象有新的引用时，那么ob_refcnt就会+1，对象被销毁时，ob_refcnt-1，当ob_refcnt减少为0时，此对象生命就结束了。ob_refcnt指的是有多少个对象使用此引用，**代表指针数**。
#### 引用计数的优缺点
>优点：
>>1.高效
>>2.实时性。可以将回收时间分散到平时，不用等到回收的特定时机。
>
>缺点：
>>引用计数小号资源
>>循环引用，会导致内存泄漏，需要其他的回收机制。

## 标记-清除 - **==主要用来处理循环引用 #F44336==**
>是一种基于追踪回收算法的垃圾回收机制，有两阶段：
>第一阶段是对所有的活动对象打上标记；
>第二阶段是对无标记的对象进行清除。
#### 如何标记
>对象之间的引用通过指针连接在一起，因此可以以对象为节点，引用关系为边，构造有向图，通过便利有向图，来标记活动对象。**根对象就是全局变量、调用栈、寄存器**。其中可达的对象是活动对象，不可达的是未活动对象。

#### 标记清除回收主要清除一些容器变量，如list，dict，tuple，instance(实例).

## 分代回收 -**==处理遍历有向图的性能浪费(以双向链表进行维护) #F44336==**
>分代回收分为**3代**。
>每次引用一个对象，python都会将其加入到零代的双向链表中。
>随后，python会循环遍历零代列表的每个对象，检查每个对象的互相引用，并依据规则减少引用计数，进行垃圾回收，将存在的对象移到下一代(一代)。
>当出发一代的回收阈值时，再次进行遍历一代列表，减少引用计数后垃圾回收，将存在的移到下一代（二代）。
#### 分代收集基于的思想是，==新生的对象更有可能被垃圾回收，而存活更久的对象也有更高的概率继续存活 #F44336==。因此，通过这种做法，可以节约不少计算量，从而提高 Python 的性能。
