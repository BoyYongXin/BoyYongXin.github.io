---
title: 某日报搜索参数securitykey加密破解
date: 2019-12-31 15:49:35
categories: python爬虫
tags: [爬虫,反编译,Android逆向] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

感觉好久没发文章了，最近小编基本处于放养状态，言归正传，最近遇到一个app，怎么遇到的呢，还用说么，公司来活了，给你的任务

 <!--more-->

**任务要求：**

给定一批关键词，进行搜索，把搜索结果近期的数据，进行，提取、入库。



接到这个app，首先进行常规操作，配置好手机代理，进行fiddler抓包，于是乎，抓到包了，拿下来，使用postman进行模拟请求

数据返回成功，ok完事了？？

根本不可能，更换一个关键词；不换关键词，请求下一页；都不能用，分析一下请求结构

![img](https://img-blog.csdnimg.cn/20191231151029774.png)

我们看到是post请求，一起看看他的form表单

![img](https://img-blog.csdnimg.cn/20191231151224429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

如何确定那个参数是校验加密，反复抓包几次，对比信息即可，对比发现：

securitykey ：主要校验参数
longitude，latitude 经纬度，暂时不重要
province_code 时间戳，十位时间戳，后面跟000

开始下一个步骤，反编译APP，看他的源码，找到加密参数的方法

小坑出现了，使用apktool反编译失败，有点猫腻，jadx直接打开apk，如图所示

![img](https://img-blog.csdnimg.cn/20191231152321514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

不看不知道，一看吓一跳，腾讯和360加固，有加固，需要脱壳

脱壳成功后，把所有脱出来的classs.dex文件，全部拿到，对比一下，先拿文件最大的下手

![img](https://img-blog.csdnimg.cn/20191231152730740.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

全局搜索，加密参数，我们找到了这个文件，瞧一瞧是不是咱所需要的

![img](https://img-blog.csdnimg.cn/20191231152951945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

如标注位置，发现正是咱们所需要的，加密算法调用，进入getMD5Str方法里面去，看看是如何加密的

![img](https://img-blog.csdnimg.cn/20191231153145614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

看一看，瞧一瞧，hook这个方法，看看传进来的是什么参数

![img](https://img-blog.csdnimg.cn/20191231153415752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0JpZ0JveV9Db2Rlcg==,size_16,color_FFFFFF,t_70)

跟据多次堆栈和print打印的结果，发现发参数基本就是这个了

有几个参数是变化的，标注一下

经纬度，页码，时间戳

这个app到这里基本就差不多，破解了，

本想着直接python调用这段java代码呢，研究了一下（代码不多），加密代码，使用python还原一下

md5_str = hashlib.md5(security_args.encode(encoding='UTF-8')).hexdigest()

其实就是python的md5加密，哈哈

构造请求就可以了，完整代码，就不上传了
