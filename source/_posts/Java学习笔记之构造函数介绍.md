---
title: Java学习笔记之构造函数介绍
date: 2022-05-13 10:31:25
categories: JAVA
tags: [java]
---

hello 大家好我是Monday，今天我们开启回补java的学习的系列文章之构造函数介绍。



<!--more-->

我们人出生的时候，有些人一出生之后再起名字的，但是有些人一旦出生就已经起好名字的。那么我们在 java 里面怎么在对象一旦创建就赋值呢？

```java
public class Person {

String name; //    姓名
int age; //    年龄
public static void main(String[] args) {
    Person p = new Person(); //    创建了Person类型的p对象
    System.out.println("姓名: " + p.name + " 年龄: " + p.age); //    name = null, age = 0;
    //这个小孩刚出生的时候没有姓名和年龄
}
```


那我们怎么在出生后给小孩起名字呢：

看看我们如何构造的：

```java
public class Person {

String name; //    姓名
int age; //    年龄
//    构造方法
Person(String name,int age){
    this.name = name; //     给对象赋予name值
    this.age = age; //    给对象赋予age值
}
public static void main(String[] args) {
    Person p = new Person("张三",1); //    创建了Person类型的p对象,并且调用构造方法赋予该对象属性值
    System.out.println("姓名: " + p.name + " 年龄: " + p.age); //    name = 张三, age = 1;
    //这个小孩刚出生的时候已经有了姓名和年龄
}
```

**到这里我们引出构造方法的作用：**

1).创建对象,凡是必须和new一起使用.

2).对对象进行初始化.

 

```java
public class Person {

String name; //    姓名
int age; //    年龄
//    全参构造方法
Person(String name,int age){
    this.name = name; //     给对象赋予name值
    this.age = age; //    给对象赋予age值
}
//    无参构造方法
Person(){
    
}

public static void main(String[] args) {
    Person p = new Person("张三",1); 
    /*
         根据创建对象的实参个数,jvm回去寻找合适的构造方法,
         两个实参所有会调用含有两个参数的构造方法.Person(String name,int age)
     */
    System.out.println("姓名: " + p.name + " 年龄: " + p.age); //    name = 张三, age = 1;
    //这个对象创建出来的时候已经有了自己的姓名和年龄
}
```


**构造函数与普通函数的区别：**

 

（1）. 一般函数是用于定义对象应该具备的功能。而构造函数定义的是，对象在调用功能之前，在建立时，应该具备的一些内容。也就是对象的初始化内容。

（2）. 构造函数是在对象建立时由 jvm 调用, 给对象初始化。一般函数是对象建立后，当对象调用该功能时才会执行。

（3）. 普通函数可以使用对象多次调用，构造函数就在创建对象时调用。

（4）. 构造函数的函数名要与类名一样，而普通的函数只要符合标识符的命名规则即可。

（5）. 构造函数没有返回值类型。

 

**构造函数要注意的细节：**

（1）. 当类中没有定义构造函数时，系统会指定给该类加上一个空参数的构造函数。这个是类中默认的构造函数。当类中如果自定义了构造函数，这时默认的构造函数就没有了。

备注：可以通过 javap 命令验证。

（2）. 在一个类中可以定义多个构造函数，以进行不同的初始化。多个构造函数存在于类中，是以重载的形式体现的。因为构造函数的名称都相同。

 

**一段整合练习的代码：综合运用的以上所涉及的知识点，还有一点点的知识扩展：**

```
package com;
import java.util.Random;
public class Construction_demo1 {
    public String name; //    姓名
    public int age; //    年龄
    Construction_demo1() {
        System.out.println("无参构造方法");
    };
    Construction_demo1(String name, int age) {
        this.age = age;
        this.name = name;
    };
    {
        cry();
    }
    //构造代码块，构造代码块和构造函数的区别，构造代码块是给所有对象进行统一初始化， 构造函数给对应的对象初始化。
    //构造代码块的作用：它的作用就是将所有构造方法中公共的信息进行抽取。
    public void cry() {
        System.out.println("----------cry------------------");
    }
    public static void main(String[] args) {
        Construction_demo1 p = new Construction_demo1(); //    创建了Person类型的p对象
        System.out.println("姓名: " + p.name + " 年龄: " + p.age); //    name = null, age = 0;
        //这个小孩刚出生的时候没有姓名和年龄
        Construction_demo1 p2 = new Construction_demo1("xiaoli", 48);
        System.out.println("姓名 ：" + p2.name + "年龄： " + p2.age);
        int result = get_nonce(0, 100); //如何调用静态方法
        System.out.println(result);
        p2.cry(); //如何调用类里面的方法
    }
    public static int get_nonce(int i, int i2) {
        if (i > i2) {
            return 0;
        }
        return i != i2 ? i + new Random().nextInt(i2 - i) : i;
    }
}
```


到这里java 的构造函数我们就告一段落了，

原文链接：https://blog.csdn.net/BigBoy_Coder/article/details/102939143

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./share/微信.png" style="zoom: 50%;" />