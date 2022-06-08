---
title: Java学习笔记之泛型介绍
date: 2022-05-12 22:48:03
categories: java
tags: [java]
---

hello 大家好我是Monday，今天我们开启SpringBoot的学习的系列文章之SpringBoot项目引入本地Jar包。



<!--more-->



**java 中泛型标记符：**

- **E** - Element (在集合中使用，因为集合中存放的是元素)

- **T** - Type（Java 类）

- **K** - Key（键）

- **V** - Value（值）

- **N** - Number（数值类型）

- **？** - 表示不确定的 java 类型

  

### 实例

下面的例子演示了如何使用泛型方法打印不同类型的数组元素：

```java
package fanxing;
/*
* java 泛型
* */

public class GenericMethodTest
{
    // 泛型方法 printArray
    public static < E > void printArray( E[] inputArray )
    {
        // 输出数组元素
        for ( E element : inputArray ){
            System.out.printf( "%s ", element );
        }
        System.out.println();
    }

    public static void main( String args[] )
    {
        // 创建不同类型数组： Integer, Double 和 Character
        Integer[] intArray = { 1, 2, 3, 4, 5 };
        Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
        Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };

        System.out.println( "整型数组元素为:" );
        printArray( intArray  ); // 传递一个整型数组

        System.out.println( "\n双精度型数组元素为:" );
        printArray( doubleArray ); // 传递一个双精度型数组

        System.out.println( "\n字符型数组元素为:" );
        printArray( charArray ); // 传递一个字符型数组
    }
}

```

运行结果如下所示：

```
整型数组元素为:
1 2 3 4 5 

双精度型数组元素为:
1.1 2.2 3.3 4.4 

字符型数组元素为:
H E L L O 

```











**参考文献：**

​	https://www.runoob.com/java/java-generics.html

​	https://www.cnblogs.com/coprince/p/8603492.html

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./share/微信.png" style="zoom: 50%;" />