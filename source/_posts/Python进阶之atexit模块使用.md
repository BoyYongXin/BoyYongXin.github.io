---
title: Python进阶之atexit模块使用
date: 2021-06-12 22:06:23
categories: python基础
tags: [python基础] #文章标签，可空，多标签请用格式，注意:后面有个空格
---





如何让程序退出时强行执行一段代码，说起这个需求，我们就不得不说**Python atexit**模块了：

<!--more-->



**退出处理器** `atexit` 模块定义了清理函数的注册和反注册函数. 被注册的函数会在解释器正常终止时执行. `atexit` 会按照注册顺序的*逆序*执行; 如果你注册了 `A`, `B` 和 `C`, 那么在解释器终止时会依序执行 `C`, `B`, `A`.

看完这段介绍，有点类似栈的原理，后进先出



**1、举个例子说明：**

```
def goodbye(name, adjective):
   print("Goodbye %s, it was %s to meet you."% (name, adjective))

def hello():
   print('hello world!')

def a():
   print('a')

import atexit
atexit.register(goodbye, 'Mr.Yang', 'nice')#3
atexit.register(a)#2
hello()#1正常程序首先被执行

#执行顺序 1 2 3
```

**运行结果**



![图片](.\Python进阶之atexit模块使用\1.jpg)



**2、 作为 [decorator]: 使用，使用场景， 但是只适用于没有参数时调用**

举例说明，对上述代码稍作修改：

```
import atexit

def hello():
   print('hello world!')
@atexit.register
def a():
   print('a')

hello()

#运行结果
#hello world!
#a
```

 **3、取消注册， 示例如下。** 

```
def goodbye(name, adjective):
   print("Goodbye %s, it was %s to meet you."% (name, adjective))

def hello():
   print('hello world!')

def a():
   print('a')

import atexit
atexit.register(goodbye, 'Mr.Yang', 'nice')#3
atexit.register(a)#2
hello()#1正常程序首先被执行
atexit.unregister(a)
#正常执行顺序 1 2 3

#由于设置了取消注册atexit.unregister(a)，

#注销后执行顺序 1 3
```

**运行结果：**

![图片](.\Python进阶之atexit模块使用\2.jpg)

这个模块一般用来在程序结束时，做资源清理。

**应用场景一：**

 既能让程序报错，又能在报错已经还能运行`clean()`呢？

```
import atexit

@atexit.register
def clean():
   print('清理环境相关的代码')

def test():
   example = {"a": 1, "b": 2}
   print(example["c"])#程序报错
test()
```

运行结果：

  

![图片](.\Python进阶之atexit模块使用\3.jpg)





这样一来，我们不需要显式调用`clean`函数了。无论程序正常结束，还是程序异常报错，`clean`函数里面的内容总会执行。





***\**官方文档\**：https://docs.python.org/zh-cn/3.7/library/atexit.html\*** 

****参考链接：\***https://mp.weixin.qq.com/s/lNwSBhcp9ktwgaGWpXNq-A*