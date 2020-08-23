---
title: python问题3
data: 2020-8-1612:50:18
tags:
 - python
 - 求职
categories:
 - 求职
mathjax: true
---
# 拷贝
>有引用、浅拷贝和深拷贝三种

``` python
import copy
a = [1, 2, 3, 4, ['a', 'b']]  #原始对象

b = a  #赋值，传对象的引用
c = copy.copy(a)  #对象拷贝，浅拷贝
d = copy.deepcopy(a)  #对象拷贝，深拷贝

a.append(5)  #修改对象a
a[4].append('c')  #修改对象a中的['a', 'b']数组对象

print 'a = ', a
print 'b = ', b
print 'c = ', c
print 'd = ', d

输出结果：
a =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
b =  [1, 2, 3, 4, ['a', 'b', 'c'], 5]
c =  [1, 2, 3, 4, ['a', 'b', 'c']]
d =  [1, 2, 3, 4, ['a', 'b']]
```
>>如果变量是不可变类型(数字，字符串，布尔，元组)，无论是引用赋值，浅拷贝，深拷贝，都会另外开辟一个新的地址进行存储，所以原变量改变不影响新变量。
>>如果是可变类型的变量(列表，字典)，那么需要分引用，浅拷贝，深拷贝三种情况分别考虑。
## 引用赋值
>对于可变类型变量,变量名存储的是此变量值所在的地址,所有使用引用赋值得到的新变量,新变量的名也是存储的原变量的地址,因此原变量的改变会影响新变量,新变量的改变也会影响原变量
## 浅拷贝
>浅拷贝只是**复制了原变量的父对象**,对父对象进行重新开辟地址,不会复制子对象,也就是子对象还是用原地址.

>比如列表a = [1,2,3,['a','b','c']],在执行浅拷贝后得到新的列表b,其中b的父对象b[0],b[1],b[2],b[3]的地址与a[0],a[1],a[2],a[3]完全不同,但是b的子对象b[3][0],b[3][1],b[3][2]与a的子对象a[3][0],a[3][1],a[3][2]公用一个地址;即a的父对象发生变化,不会影响b的父对象,但是a的子对象发生变化,就会影响b的子对象.
具体可看下图:
[![浅拷贝](https://s1.ax1x.com/2020/08/17/dmrmlR.png)](https://imgchr.com/i/dmrmlR)
## 深拷贝
>深拷贝是完全复制原变量的所有,另开辟新的地址存储,所有的对象都有一个新地址,所以不论怎么改变,都不会影响深拷贝后变量的改变.

# is 和 == 的区别
>is 是对地址进行比较,也就是变量的id,== 是对 变量的值进行比较,具体可以看下面代码

``` python
a=[1,2,3,"abc"]
b=[1,2,3,"abc"]

print("a=",a)
print("b=",b)
print("a的地址",id(a))
print("b的地址",id(b))
print("==比较(比较值)",a==b)
print("is 比较(比较地址)",a is b)
```
>结果是:
>a= [1, 2, 3, 'abc']
b= [1, 2, 3, 'abc']
a的地址 2014214530632
b的地址 2014214345416
==比较(比较值) True
is 比较(比较地址) False

# read() ,readline(),readlines() 
>
read()  ： 一次性读取整个文件内容。推荐使用read(size)方法，size越大运行时间越长
readline()  ：每次读取一行内容。内存不够时使用，一般不太用
readlines()   ：一次性读取整个文件内容，并按行返回到list，方便我们遍历

>都是对文件的读入,read()和readlines()都是读取文件所有的内容,其中read()是读取后按源文件内容格式存储为字符串格式.readlines()是按行存储为list格式
>readline()不常用,是按行读取,每次只读取一行,在大文件中进行操作,用完一行内容在读取下一行.

# Python 2.7.x 与 Python 3.x 的主要差异

 1. print的差异
 2. 2.x中使用raw_input()输入,3.x中是input()
 3.  Python2.x中有xrange()和range(),xrange()是惰性机制的，如果只循环一次建议使用range(),对此的话range()会在内存中创建多尔列表，内存开销较大。python3中只有range()，range有了一个新的__contains__方法。**__contains__方法可以有效的加快Python 3.x中整数和布尔型的“查找”速度**。
 4.  异常处理，在python3.x中必须使用‘as‘来处理， python2.x中可以不必使用
 5.  在python2.x 中.Next()函数可以作为函数的属性使用，也可以单独作为函数使用；在python3.x 中只能作为函数使用。Next()会触发attributeError.
 6.  Python 2有基于ASCII的str()类型，其可通过单独的unicode()函数转成unicode类型，但**没有byte类型**。在Python 3中，终于有了Unicode（utf-8）字符串，以及两个字节类：**bytes和bytearrays**。

# super().__init__()
>这是使用在类的继承上的,在类的继承上分为三种状况
>1.直接继承,不要__init__(),此方法自动继承父类的属性
>2.采用__init__(),此方法在继承类后得到的新类并没有父类的属性
>3.采用super().__init__(),继承父类的属性,并可以对属性值进行修改

``` python
class A(object):
    def __init__(self,name = "person"):
        self.name = name
class B(A):
    pass
class B_init(A):
    def __init__(self,age):
        self.age = age

class B_superinit(A):
    def __init__(self,name,age):
        self.age = age
        super().__init__(name)
```
输出上:
``` python
>>g = A()
>>print(g.name)
person
>>g = B()
>>print(g.name)
person
>>g = B_init(10)
>>print(g.name)
AttributeError: 'B_init' object has no attribute 'name'
>>g = B_superinit("B_superinit",10)
>>print(g.name)
B_superinit
```

