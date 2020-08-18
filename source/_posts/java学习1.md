---
title: java学习1
data: 2020-8-17 12:50:18
tags:
 - java
 - 求职
categories:
 - 求职
mathjax: true
---
# Java中的构造方法
>1.**构造方法和类名相同**
>2.构造方法**没有返回值的类型的声明(int,void**)
>3.在创建对象时,需要**调用new**
>4.如果没有定义构造方法,在调用类时会**自动添加**无参数的构造方法
>5.构造方法**不能return**
>6.如果有多个构造方法,类会**自动根据不同的参数选择相应的构造方法**
>7.如果定义了**有参或无参的构造方法**,类**不会**再自动添加无参的构造方法

## 无参
``` java
class preson{
    public preson(){
        System.out.println("调用无参类型的构造函数");
    }
}
class test{
    public static void main(String[] args) {
        preson p  = new preson();
    }
}
```
>输出:
>调用无参类型的构造函数

## 有参

``` java
class preson{
    public preson(int a ,int b){
        System.out.println("调用有参类型的构造函数");
        int c = a*b;
        System.out.println("a*b="+c);
    }
}
class test{
    public static void main(String[] args) {
        preson p  = new preson(3,5);
    }
}
```
>输出
>调用有参类型的构造函数
a*b=15

## 总结
>构造方法可以看作是对对象的初始化,在new后就对对象的初始值进行赋值;而普通方法可以看作是对象的行为或者动作,只有对象对此方法进行操作时,才会调用.

# Java中的变量
## 类变量
>被static修饰过的变量时java中的类变量,首先会执行这些类变量.
>类变量也称为静态变量，在类中以 **static** 关键字声明，但**必须在方法之外**。
>无论一个类被new了多少个对象**,他的类变量只有一份**,也就是说,一个对象改变类变量,其他对象的此类变量也会发生改变.
>静态变量在第一次被访问时创建，在程序结束时销毁。
>默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
>
## 实例变量
>没有任何修饰的,并且在方法,构造方法,程序块之外的变量称为实例变量.它有以下特点:
>1.当一个对象被实例化之后，每个实例变量的值就跟着确定
>实例变量在对象创建的时候创建，在对象被销毁的时候销毁；
>**访问修饰符可以修饰实例变量**；
>实例变量对于**同类中**的方法、构造方法或者语句块是**可见的**。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
>**实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false**，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；

## 局部变量
>在方法,构造方法,程序块中定义的变量是局部变量,这些变量在使用方法,构造方法和程序块时采用被声明和赋值,在方法,构造方法,程序块结束时局部变量被销毁.它有以下特点:
>1.访问修饰符**不能用于局部变量**；
>2.局部变量的作用域只在其定义的方法,构造方法,程序块中,在外部不可见.
>3.局部变量是在**栈**上分配的。
>4.局部变量**必需经过声明和初始化**才可以使用.

## 示例代码:

``` java
class preson{
    static int a;//定义类变量,可以添加访问修饰符,一般为public
    int b = 4;      //定义实例变量,可以添加访问修饰符,如果为private,只能对继承的子类可见.
    public preson(String x){
        String S = "定义新对象";//定义局部变量
        System.out.println(S+x+a);
    }
}
class test{
    public static void main(String[] args) {
        preson p  = new preson("p");
        p.a = 5;//此处修改类变量a,在q对象中也会发生改变
        p.b = 6;//此处修改实例变量b,只会作用在p对象上,q对象不会改变
        System.out.println(p.a);
        System.out.println(p.b);
        preson q = new preson("q");
        System.out.println(q.a);
        System.out.println(q.b);
    }
}
```
>结果:
>定义新对象p0
5
6
定义新对象q5
5
4

# Java修饰符
>分为访问修饰符和非访问修饰两种
饰符用来定义类、方法或者变量，通常放在语句的最前端。我们通过下面的例子来说明：

``` java
public class ClassName {
   // ...
}
private boolean myFlag;//私有实例变量,只自身和子类可见
static final double weeks = 9.5;//final修饰符,是为常量,不可更改
protected static final int BOXWIDTH = 42;
public static void main(String[] arguments) {//主函数入口
   // 方法体
}
```

## 访问修饰符
>有四类访问修饰符来控制变量,方法的访问,default,public,private,protected.
default访问修饰符即在变量,方法之前什么都不写,是指在同一个包内可见,使用对象：类、接口、变量、方法。
[![访问修饰限制](https://s1.ax1x.com/2020/08/17/dnnwE8.png)](https://imgchr.com/i/dnnwE8)
### default
>使用默认访问修饰符声明的变量和方法，**对同一个包内的类是可见的**。使用default修饰的接口,其接口里的**变量**都隐式声明为 ==public static final #F44336==,而接口里的**方法**默认情况下访问权限为 ==public #F44336==。
如下例所示，变量和方法的声明可以不使用任何修饰符。

``` java
String version = "1.5.1";
boolean processOrder() {
   return true;
}
```
### public
>公有访问修饰符,被public修饰的变量,方法,类,接口对所有的包都是可见的,也就是能被任何类都访问.
>Java 程序的 **==main() 方法必须设置成公有的 #F44336==**，否则，Java 解释器将不能运行该类。

### projected
>受保护的访问修饰符;
>projected需要分为两种情况讨论:
>1.子类和父类都在同一个包中,那么这个包中的其他类都能访问projected方法和变量
>2.子类和父类不在同一个包中,那么父类包和子类都能访问project方法和变量,但是其他类不能访问.

### private
>私有访问修饰符是最严格的访问级别，所以被声明为 private 的方法、变量和构造方法只能被所属类访问，并且类和接口不能声明为 private。
声明为私有访问类型的变量只能通过类中**公共的 getter 方法被外部类访问**。
Private 访问修饰符的使用主要**用来隐藏类的实现细节和保护类的数据**。