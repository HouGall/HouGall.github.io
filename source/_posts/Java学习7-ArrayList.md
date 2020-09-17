---
title: Java学习7-ArrayList
data: 2020-9-11 16:50:18
tags:
 - java
 - 求职
 - 操作系统
categories:
 - 求职
mathjax: true
---

## ArrayList相关

### 构造函数

#### ArrayList的三种初始化

```java
/**
     * 默认初始容量大小
     */
    private static final int DEFAULT_CAPACITY = 10;//默认容量
    

    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};//定义空数组

    /**
     *默认构造函数，使用初始容量10构造一个空列表(无参数构造)
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;//无参构造，初始化为空数组
    }
    
    /**
     * 带初始容量参数的构造函数。（用户自己指定容量）
     */
    public ArrayList(int initialCapacity) {//有参构造，用户自己输入构造的容量，进行初始化
        if (initialCapacity > 0) {//初始容量大于0
            //创建initialCapacity大小的数组
            this.elementData = new Object[initialCapacity];//定义一个用户自己定义的长度的object数组
        } else if (initialCapacity == 0) {//初始容量等于0
            //创建空数组
            this.elementData = EMPTY_ELEMENTDATA;
        } else {//初始容量小于0，抛出异常
            throw new IllegalArgumentException("Illegal Capacity: "+ //小于0，抛出异常
                                               initialCapacity);
        }
    }


   /**
    *构造包含指定collection元素的列表，这些元素利用该集合的迭代器按顺序返回
    *如果指定的集合为null，throws NullPointerException。 
    */
     public ArrayList(Collection<? extends E> c) {//构造泛型的数据
        elementData = c.toArray();//先将数据转为数组
        if ((size = elementData.length) != 0) {//定义size的长度，后面会用到，并判断是不是0
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)//判断加入的泛型类型是不是elementdata的类型
                elementData = Arrays.copyOf(elementData, size, Object[].class);//类型不一致，就进行类型转换，重新分配一个加入数据的类型拷贝
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;//长度是0，还是返回一个空数组
        }
    }
```

#### 初始化后的添加元素和扩容(以无参构造为例)

###### 添加元素

```java
    /**
     * 将指定的元素追加到此列表的末尾。 
     */
    public boolean add(E e) {
   //添加元素之前，先调用ensureCapacityInternal方法
        ensureCapacityInternal(size + 1);  // 这个方法会判断容量是否够用，如果不够用会进行扩容
        //这里看到ArrayList添加元素的实质就相当于为数组赋值
        elementData[size++] = e;
        return true;
    }
```



###### ensureCapacityInternal（size+1）方法

```java
//得到最小扩容量
    private void ensureCapacityInternal(int minCapacity) {//minCapacity=size
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
              // 获取默认的容量和传入参数的较大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);//获取与默认值的最大值
            //第一个元素加入前，可以看到mincapacity变成了10
        }

        ensureExplicitCapacity(minCapacity);//调用函数进行判断是否扩容
    }
```

###### ensureExplicitCapacity(minCapacity)方法

```java
//判断是否需要扩容
    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)//最小容量已经不能满足加入元素的容量了，需要扩容
            //当mincapacity在10以内时，不会进入这里，也就不会扩容
            //调用grow方法进行扩容，调用此方法代表已经开始扩容了
            grow(minCapacity);//调用这个方法进行扩容
    }
```

###### grow(minCapacity)

```java
 /**
     * 要分配的最大数组大小
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;//能分配的最大容量

    /**
     * ArrayList扩容的核心方法。
     */
    private void grow(int minCapacity) {
        // oldCapacity为旧容量，newCapacity为新容量
        int oldCapacity = elementData.length;//旧的容量
        //将oldCapacity 右移一位，其效果相当于oldCapacity /2，
        //我们知道位运算的速度远远快于整除运算，整句运算式的结果就是将新容量更新为旧容量的1.5倍，
        int newCapacity = oldCapacity + (oldCapacity >> 1);//新容量是旧容量的1.5倍
        //然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量，
        if (newCapacity - minCapacity < 0)//新容量不够最小的容量时，将需要的最小容量给新容量。
            newCapacity = minCapacity;
       // 如果新容量大于 MAX_ARRAY_SIZE,进入(执行) `hugeCapacity()` 方法来比较 minCapacity 和 MAX_ARRAY_SIZE，
       //如果minCapacity大于最大容量，则新容量则为`Integer.MAX_VALUE`，否则，新容量大小则为 MAX_ARRAY_SIZE 即为 `Integer.MAX_VALUE - 8`。
        if (newCapacity - MAX_ARRAY_SIZE > 0)//新容量超出了最大容量
            newCapacity = hugeCapacity(minCapacity);//对新容量重新分配，给最大容量或者2^32-1.
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);//旧的数组拷贝到新的扩容数组中
    }
```



