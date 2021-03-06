---
title: Java学习3-单例模式
data: 2020-9-10 14:50:18
tags:
 - java
 - 求职
categories:
 - 求职
mathjax: true
---
# 单例模式

> 单例模式是指一个类只有一个实例对象，并且提供一个全局访问点。
>
> 优点是只有一个类对象，节约资源，较少开销，提高系统效率。
>
> 缺点是只有一个实例，导致单例类的职责过重。
>
> ###### 单例模式属于三大类的创建型模式
>
> 具有三个特点：
>
> 1.只有一个类的实例对象。
>
> 2.实现自我实例化。
>
> 3.提供一个全局访问点。
>
> 常见的单例模式有五类：饿汉式，懒汉式，双重检测锁式，静态内部类式，枚举单例。

#### 饿汉式

> 线程安全，调用效率高，但是不能延时加载。
>
> ```java
> public class SingletonDemo1 {
> 
>     //线程安全的
>     //类初始化时，立即加载这个对象
>     private static SingletonDemo1 instance = new SingletonDemo1();
> 
>     private SingletonDemo1() {
>     }
> 
>     //方法没有加同步块，所以它效率高
>     public static SingletonDemo1 getInstance() {
>         return instance;
>     }
> }
> ```
>
> 由于此类单例模式在类加载时已经创建了实例对象，所以类加载慢，但是调用对象快，并且线程安全。

#### 懒汉式

> 线程不安全
>
> ```java
> public class SingletonDemo1 {
>     //懒汉式.线程不安全
>     private static SingletonDemo1 instance = null;
>     private SingletonDemo1(){
>         //无参空白的构造函数
>     }
>     public static SingletonDemo1 getInstance(){//提供一个全局访问点
>         if (instance==null){
>             instance = new SingletonDemo1();
>         }
>         return instance;
>     }
> }
> ```
>
> 此类是在运行时才会创建实例对象，所以类加载快，但是在调用时比较慢。并且线程不安全，如果要线程安全需要加上synchronized关键字。

#### 双重检测锁式（加锁的懒汉式）

>线程安全，双重检测，进行实例化类.
>
>```java
>public class SingletonDemo1{
>    //双重检测锁式
>    private static SingletonDemo1 instance = null;
>    private SingletonDemo1(){
>        //构造方法
>    }
>    public static SingletonDemo1 getInstance(){
>        if (instance == null){//第一重检测
>            synchronized (SingletonDemo1.class){
>                if (instance==null){//第二重检测
>                    instance = new SingletonDemo1();
>                }
>            }
>        }
>        return instance;
>    }
>}
>```

##### 注意：单例类的构造方法都是私有，也就是说不能被继承，同时由于私有，不能被外部类实例化。