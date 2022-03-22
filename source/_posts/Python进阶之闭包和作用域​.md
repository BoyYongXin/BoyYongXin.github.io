---
title: Python进阶之闭包和作用域
date: 2021-06-09 21:57:55
categories: python基础
tags: [python基础, 面试集锦] #文章标签，可空，多标签请用格式，注意:后面有个空格
---



**前言：**

好久没写文章了，今天来给大家更新一篇，最近会陆续整理，发出一下以前做的笔记

<!--more-->



**作用域**：

```python
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

Python 中只有模块（module），类（class）以及函数（def、lambda）才会引入新的作用域，其它的代码块

（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，

如下代码：



```python

if True:
    msg = 'I am from beijing'
print(msg)
//运行结果 I am from beijing

```

**全局变量和局部变量**

```python
total = 0  # 全局变量

def sum(arg1, arg2):
    total = arg1 + arg2  # total在这里是局部变量.
    return total
print(total)#函数外是全局变量total
total = sum(10, 20)
print(total)#局部变量total

```



### global 和 nonlocal关键字

当内部作用域想修改外部作用域的变量时

```python
num = 1
def fun1():
    global num  # 需要使用 global 关键字声明
    print(num) 
    num = 123
    print(num)
fun1()
print(num)
#结果
#1
#123
#123
```



如果要修改嵌套作用域（非全局作用域）中的变量则需要 nonlocal 关键字了 ，仅读取就无所谓了

```python

def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax += n
        return ax

    return sum

s = lazy_sum(1, 2, 3, 4, 5)
print(s, type(s),s(),sep='\n')


# Output>>>
<function lazy_sum.<locals>.sum at 0x10768ed90>
<class 'function'>
15
```



**进入正题：**

**闭包**

 在函数嵌套的程序结构中，如果内层函数包含对外层函数局部变量的引用，同时外层函数的返回结果又是对内层函数的引用，这就构成了一个闭包。

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax += n
        return ax

    return sum

s = lazy_sum(1, 2, 3, 4, 5)
print(s, type(s),s(),sep='\n')


# Output>>>
<function lazy_sum.<locals>.sum at 0x10768ed90>
<class 'function'>
15
```

在上面的代码中，我们在外层函数`lazy_sum`中又嵌套了的内层函数`sum`，`sum`引用了`lazy_sum`的参数，并且`lazy_sum` 将`sum` 作为返回结果。如果是按照命令式语言的规则（如C++，C#），在执行`sum`函数时，会由于在其作用域内找不到`args`变量而出错，但是在函数式语言中，**当内嵌的函数体内有引用外部作用域的变量时，将会把定义时涉及到的引用环境和函数体打包成一个整体返回**，这中程序结构就是上面说的"闭包(Closure)"。所以说，闭包就是由函数及其相关的引用环境组合而成的实体，即：闭包=函数+引用环境。

从运行结果中可以看到，当调用`lazy_sum(*args)`时，返回的是内部求和函数的引用地址，当调用函数`sum()` 时，才真正计算求和的结果。

```python

s1 = lazy_sum(1, 2, 3, 4, 5)
s2 = lazy_sum(1, 2, 3, 4, 5)
print(s1==s2)

# Output>>>
False
```



我们使 用`nonlocal` 关键字 对上面的代码进行更改一下

```python

def lazy_sum(*args):
    ax = 0
    def sum():
        nonlocal ax
        for n in args:
            ax += n
        return ax
    return sum

s = lazy_sum(1, 2, 3, 4, 5)
print(s, type(s), s(), sep='\n')

# Output>>>
<function lazy_sum.<locals>.sum at 0x000001B0D80612F0>
<class 'function'>
15
```

**闭包的应用场景：**

```python
class UrlTemplate:
    def __init__(self, template):
        self.template = template

    def openr(self, **kwargs):
        return self.template.format_map(kwargs)
    

# 上面的类可以被一个更简单的函数代替
def url_template(template):
    def openr(**kwargs):
        return template.format_map(kwargs)
    return openr

root_url = url_template('https://www.jianshu.com/search?q={name}&page={page}&type={type}')

print(root_url(name='sss', page='1', type='note'))
```



**总结**

使用一个内部函数或者闭包的方案通常会更优雅一些。简单来讲，一个闭包就是一个函数，只不过在函数内部带上了一个额外的变量环境。闭包关键特点就是它会记住自己被定义时的环境。因此，在我们的解决方案中，opener() 函数记住了template参数的值，并在接下来的调用中使用它。任何时候只要碰到需要给某个函数增加额外的状态信息的问题，都可以考虑使用闭包。相比将函数转换成一个类而言，闭包通常是一种更加简洁和优雅的方案。



***参考链接：\***

*https://www.jianshu.com/p/77d78d84a69c*