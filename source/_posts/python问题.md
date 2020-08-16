---
title: python问题
data: 2020-8-15 12:50:18
tags:
 - python
 - 求职
categories:
 - 求职
mathjax: true
---
# 新式类和经典类的代码区别

``` python
#新式类
class C(object):
    pass
#经典类
class B:
    pass
```
>简单的说，新式类是在创建的时候继承内置object对象（或者是从内置类型，如list,dict等）;
>而经典类是直接声明的。使用dir()方法也可以看出新式类中定义很多新的属性和方法，而经典类好像就2个：
>[![新式类和经典类dir()区别](https://s1.ax1x.com/2020/08/15/dFtyHf.png)](https://imgchr.com/i/dFtyHf)
这些新的属性和方法都是从object对象中继承过来的。

# 单例模式
>单例模式是一种常用的软件设计模式。在它的核心结构中**只包含一个被称为单例类的特殊类**。通过单例模式可以**保证系统中一个类只有一个实例而且该实例易于外界访问**，从而方便对实例个数的控制并**节约系统资源**。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。

## 单例模式创建方法
### 方法一：使用__new__方法

``` python
class Singleton(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kw)
        return cls._instance

class MyClass(Singleton):
    a = 1
```

### 方法二： 共享属性
>创建实例时把所有实例的__dict__指向同一个字典,这样它们具有相同的属性和方法.

``` python
class Borg(object):
    _state = {}
    def __new__(cls, *args, **kw):
        ob = super(Borg, cls).__new__(cls, *args, **kw)
        ob.__dict__ = cls._state
        return ob

class MyClass2(Borg):
    a = 1
```

### 方法三：装饰器版本

``` python
def singleton(cls):
    instances = {}
    def getinstance(*args, **kw):
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return getinstance

@singleton
class MyClass:
  ...
```
### 方法四： import方法
>作为python的模块是天然的单例模式

``` python
# mysingleton.py
class My_Singleton(object):
    def foo(self):
        pass

my_singleton = My_Singleton()

# to use
from mysingleton import my_singleton

my_singleton.foo()
```

# 命名空间和作用域
## 命名空间
>命名空间提供了在项目中避免名字冲突的一种方法。各个命名空间是独立的，没有任何关系的，所以一个命名空间中不能有重名，但不同的命名空间是可以重名而没有任何影响。
我们举一个计算机系统中的例子，一个文件夹(目录)中可以包含多个文件夹，每个文件夹中不能有相同的文件名，但不同文件夹中的文件可以重名。
[![文件夹文件示例](https://s1.ax1x.com/2020/08/15/dFB4FP.jpg)](https://imgchr.com/i/dFB4FP)

一般有三种命名空间：
- 内置名称（built-in names）， Python 语言内置的名称，比如函数名 abs、char 和异常名称 BaseException、Exception 等等。
- 全局名称（global names），模块中定义的名称，记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。
- 局部名称（local names），函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）
[![三种命名空间关系](https://s1.ax1x.com/2020/08/15/dFDpSU.png)](https://imgchr.com/i/dFDpSU)
  
  >命名空间查找顺序:
假设我们要使用变量 runoob，则 Python 的查找顺序为：局部的命名空间去 -> 全局命名空间 -> 内置命名空间。
如果找不到变量 runoob，它将放弃查找并引发一个 NameError 异常:

``` python
NameError: name 'runoob' is not defined。

```
>命名空间的生命周期：
命名空间的生命周期取决于对象的作用域，如果对象执行完成，则该命名空间的生命周期就结束。
因此，我们无法从外部命名空间访问内部命名空间的对象。

## 作用域
>作用域就是一个 Python 程序可以直接访问命名空间的正文区域。
在一个 python 程序中，直接访问一个变量，会从内到外依次访问所有的作用域直到找到，否则会报未定义的错误。
Python 中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。

变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。Python的作用域一共有4种，分别是：
有四种作用域：
- L（Local）：最内层，包含局部变量，比如一个函数/方法内部。
- E（Enclosing）：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类）  A 里面又包含了一个函数 B ，那么对于 B 中的名称来说 A 中的作用域就为 nonlocal。
- G（Global）：当前脚本的最外层，比如当前模块的全局变量。
- B（Built-in）： 包含了内建的变量/关键字等。，最后被搜索

>在局部作用域找不到，便会去局部外的局部找（例如闭包），再找不到就会去全局找，再者去内置中找。

``` python
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```
>Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：

``` python
>>> if True:
...  msg = 'I am from Runoob'
... 
>>> msg
'I am from Runoob'
>>> 
```
>实例中 msg 变量定义在 if 语句块中，但外部还是可以访问的。
如果将 msg 定义在函数中，则它就是局部变量，外部不能访问：

``` python
>>> def test():
...     msg_inner = 'I am from Runoob'
... 
>>> msg_inner
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'msg_inner' is not defined
```
>从报错的信息上看，说明了 msg_inner 未定义，无法使用，因为它是局部变量，只有在函数内可以使用。

### 全局变量和局部变量
>定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。
局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。

``` python
#!/usr/bin/python3
 
total = 0 # 这是一个全局变量
# 可写函数说明
def sum( arg1, arg2 ):
    #返回2个参数的和."
    total = arg1 + arg2 # total在这里是局部变量.
    print ("函数内是局部变量 : ", total)
    return total
 
#调用sum函数
sum( 10, 20 )
print ("函数外是全局变量 : ", total)
```
>结果是:
>函数内是局部变量 :  30
函数外是全局变量 :  0

## global 和 nonlocal关键字
>当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

### global

``` python
#!/usr/bin/python3
 
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)
```
>结果:
>1
123
123
### nonlocal
>如果要修改嵌套作用域（enclosing 作用域，外层非全局作用域）中的变量则需要 nonlocal 关键字了，如下实例：

``` python
#!/usr/bin/python3
 
def outer():
    num = 10
    def inner():
        nonlocal num   # nonlocal关键字声明
        num = 100
        print(num)
    inner()
    print(num)
outer()
```
>结果：
>100
100