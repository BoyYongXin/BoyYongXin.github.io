---


title: 生成器中的return有什么作用
date: 2021-06-06 18:46:50
categories: python基础
tags: [Python基础, 生成器] #文章标签，可空，多标签请用格式，注意:后面有个空格

---

生成器中的return有什么作用

<!--more-->

```python

def generate_data(num):
    if num > 10:
        for i in range(num):
            yield i
    else:
        return num
if __name__ == '__main__':
    res = generate_data(num=12)
    print(res, type(res))
    for i in res:
        print(i, type(i))
```

**运行结果：**

```
<generator object generate_data at 0x000002279C741750> <class 'generator'>
0 <class 'int'>
1 <class 'int'>
2 <class 'int'>
3 <class 'int'>
4 <class 'int'>
5 <class 'int'>
6 <class 'int'>
7 <class 'int'>
8 <class 'int'>
9 <class 'int'>
10 <class 'int'>
11 <class 'int'>
```

**今天的重点，当num值小于10时：**

```python
 res = generate_data(num=5)
    print(res, type(res))
    for i in res:
        print(i, type(i))
```



**运行结果：**

<generator object generate_data at 0x00000282B21E1750> <class 'generator'>



并没有返回5这个值,但是代码也没有报错，通过印结果，这说明返回的res 也是生成器



**带着疑问，我们进行搜索**

https://stackoverflow.com/questions/16780002/return-in-generator-together-with-yield-in-python-3-3

**这时候我们明白了：**

return 在生成器中，表示生成器运行完成了，可以结束了。然后生成器会抛出一个StopIteration的异常。而for循环能够检测到这个异常，于是结束循环。所以当我们传入的参数为5的时候，生成器直接运行到了return，于是它直接就抛出StopIteration，于是for 循环检测到这个异常就结束了。
 在生成器里面的return只是一个结束标志，它不会把后面写的值返回给调用者。这跟函数里面的return语句是不一样的.



**我们验证一下：**

```
def f():
    return 3
    yield 2
if __name__ == '__main__':
    x = f()
    print(x.__next__())
```

**运行结果：**

```
Traceback (most recent call last):
  File "E:/workerspace/demo/a.py", line 14, in <module>
    print(x.__next__())
StopIteration: 3

```

果真如此

```

```