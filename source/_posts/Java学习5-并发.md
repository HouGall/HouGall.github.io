---
title: Java学习5-并发
data: 2020-9-11 16:50:18
tags:
 - java
 - 求职
categories:
 - 求职
mathjax: true
---
# 并发常见问题整理

## 1.synchronized关键字

> synchronized关键字是解决多线程并发下访问资源的同步性，被synchronized修饰的变量和方法在被多线程访问的时候，在同一时刻只能被一个线程占用，其他线程需要等待此线程释放资源。
>
> ##### synchronized的三个用法：
>
> 1.用于修饰实例方法：
>
> > 修饰实例方法，其实就是对当前的对象(实例对象)加锁，当线程要同步此实例方法时，需要获得当前实例对象的使用权(锁)。
> >
> > ```java
> > synchronized void method() {
> >   //业务代码
> > }
> > ```
> >
> > 
>
> 2.用于修饰静态方法：
>
> > 静态方法就是类方法，所有的对象共有，所以修饰静态方法就是对这个类加了锁。所以作用域是此类所有的对象实例。当线程要同步此静态方法时，需要获得该Class类的使用权(锁)。和修饰类一样，都是作用到整个类对象。
> >
> > ```java
> > synchronized void staic method() {
> >   //业务代码
> > }
> > ```
>
> > ###### 注意：如果两个线程，一个同步synchronized修饰的实例方法，一个同步synchronized修饰的静态方法，不会发生互斥，因为一个是对当前实例的锁，另一个是对当前类的锁。
>
> 3.用于修饰代码块：
>
> > 如果修饰代码块，就是在此代码块上加了锁，表示对当前对象或类生效。如果是synchronized(thins|object)是对当前对象生效，如果是synchronized(类.class)对当前类生效。
>
> ##### 注意：尽量不要使用 synchronized(String a) ，因为 JVM 中，字符串常量池具有缓存功能！