---
title: linux安裝mitmproxy配置https证书
date: 2022-04-07 11:00:27
categories: 技术杂谈
tags: [mitmproxy, 科学上网]
---

hello 大家好我是Monday，今天和大家聊聊关于linux安裝mitmproxy配置https证书；

<!--more-->



### **1.安装mitmproxy**

下载mitmproxy[二进制](https://so.csdn.net/so/search?q=二进制&spm=1001.2101.3001.7020)安装包：

https://github.com/mitmproxy/mitmproxy/releases/

我下载的版本为mitmproxy-4.0.1-linux.tar.gz

下载之后需要解压然后将其配置到[环境变量](https://so.csdn.net/so/search?q=环境变量&spm=1001.2101.3001.7020)。

```bash
tar -zxvf mitmproxy-4.0.1-linux.tar.gz
sudo mv mitmproxy mitmdump mitmweb /usr/bin
```

也可以直接使用python pip 的方式安装

接下来需要安装证书

### **2. 证书配置**

对于 MitmProxy 来说，如果想要截获 HTTPS 请求，我们就需要设置证书，MitmProxy 在安装后会提供一套 CA 证书，只要客户端信任了 MitmProxy 提供的证书，我们就可以通过 MitmProxy 获取 HTTPS 请求的具体内容，否则 MitmProxy 是无法解析 HTTPS 请求的。

（1）首先运行一下命令产生 CA 证书，启动 MitmDump 即可：

mitmdump

**在root目录下生 成.mitmproxy** 目录里面找到 CA 证书



（2）如果在[Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020) 上如果遇到的是.pem的文件，必须先将其转换为.crt文件：

```csharp
 openssl x509 -in mitmproxy-ca-cert.pem -inform PEM -out mitmproxy-ca-cert.crt
```

（3）在以下位置为额外的CA证书创建目录/usr/share/ca-certificates：

```bash
sudo mkdir /usr/share/ca-certificates/extra
```

（4）将CA .crt文件复制到此目录：

```bash
sudo cp  mitmproxy-ca-cert.crt /usr/share/ca-certificates/extra/mitmproxy-ca-cert.crt
```

（5）让Ubuntu添加.crt文件的路径相/usr/share/ca-certificates对于/etc/ca-certificates.conf：

```undefined
sudo dpkg-reconfigure ca-certificates
```


 （6）遇到一个安装界面，直接回车就可以了，安装完成



**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./linux安裝mitmproxy配置https证书/微信.png" style="zoom: 50%;" />