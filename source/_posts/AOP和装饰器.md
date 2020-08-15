---
title:AOP和装饰器
data: 2020-8-15 12:50:18
tags:
 - python
 - 求职
categories:
 - 求职
mathjax: true
---

# AOP
>简言之、这种在运行时，编译时，类和方法加载时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。
我们管切入到指定类指定方法的代码片段称为切面，而切入到哪些类、哪些方法则叫切入点。有了AOP，我们就可以把几个类共有的代码，抽取到一个切片中，等到需要时再切入对象中去，从而改变其原有的行为。

# 装饰器
>装饰器的作用就是为已经存在的对象添加额外的功能。
>装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。

## 手写装饰器
``` python
# 装饰器就是把其他函数作为参数的函数
def my_new_decorator(a_function_to_decorate):

    # 在函数里面,装饰器在运行中定义函数: 包装.
    # 这个函数将被包装在原始函数的外面,所以可以在原始函数之前和之后执行其他代码..
    def the_wrapper_function():

        # 把要在原始函数被调用前的代码放在这里
        print "Before the function runs"

        # 调用原始函数(用括号)
        a_function_to_decorate()

        # 把要在原始函数调用后的代码放在这里
        print "After the function runs"

    # 在这里"a_function_to_decorate" 函数永远不会被执行
    # 在这里返回刚才包装过的函数
    # 在包装函数里包含要在原始函数前后执行的代码.
    return the_wrapper_function

# 加入你建了个函数,不想修改了
def a_stand_alone_function():
    print "I am a stand alone function, don't you dare modify me"

a_stand_alone_function()
#输出: I am a stand alone function, don't you dare modify me

# 现在,你可以装饰它来增加它的功能
# 把它传递给装饰器,它就会返回一个被包装过的函数.

a_function_decorated = my_new_decorator(a_stand_alone_function)
# 执行
a_function_decorated()
#输出s:
#Before the function runs
#I am a stand alone function, don't you dare modify me
#After the function runs
```
## 真实装饰器

``` python
# 装饰器就是把其他函数作为参数的函数
def my_new_decorator(a_function_to_decorate):

    # 在函数里面,装饰器在运行中定义函数: 包装.
    # 这个函数将被包装在原始函数的外面,所以可以在原始函数之前和之后执行其他代码..
    def the_wrapper_function():

        # 把要在原始函数被调用前的代码放在这里
        print "Before the function runs"

        # 调用原始函数(用括号)
        a_function_to_decorate()

        # 把要在原始函数调用后的代码放在这里
        print "After the function runs"

    # 在这里"a_function_to_decorate" 函数永远不会被执行
    # 在这里返回刚才包装过的函数
    # 在包装函数里包含要在原始函数前后执行的代码.
    return the_wrapper_function

# 加上@修饰符，包装下面的函数
@my_new_decorator
def a_stand_alone_function():
    print "I am a stand alone function, don't you dare modify me"

a_stand_alone_function()
#输出: 
#Before the function runs
#I am a stand alone function, don't you dare modify me
#After the function runs
```

