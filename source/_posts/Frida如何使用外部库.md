---
title: Frida如何使用外部库
date: 2022-04-01 21:57:51
categories: 逆向
tags: [Hook, frida, 逆向]
---

Frida  hook  时如何使用外部库介绍

<!--more-->



我们以fastjson为例



**1、下载jar包**

https://repo1.maven.org/maven2/com/alibaba/fastjson/1.2.76/

下载 fastjson.jar

**2、重新打包为dex**
进入Android SDK的目录, 执行如下命令: dx --dex --output=fastjson.dex fastjson.jar



**3、将fastjson.dex 放到手机文件里**

```
adb push /xxx/fastjson.dex /data/local/tmp
```

得到的 fastjson.dex通过adb push到手机 /data/local/tmp目录

chmod 777  /data/local/tmp/fastjson.dex

然后就可以使用fastjson了。

**4、关键相关代码：**

```javascript
Java.openClassFile('/data/local/tmp/fastjson.dex').load();
var JSONObject = Java.use('com.alibaba.fastjson.JSONObject');
JSONObject.toJSONString(obj);
```

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="/Frida如何使用外部库/微信.png" style="zoom: 50%;" />