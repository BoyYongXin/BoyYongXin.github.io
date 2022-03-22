---
title: JS解析之 某json混淆阅读思路
date: 2020-01-04 15:49:35
categories: python爬虫
tags: [爬虫,反编译,js逆向] #文章标签，可空，多标签请用格式，注意:后面有个空格
---

 目标网址：[http://www.python-spider.com](http://www.python-spider.com/)

  采集任务：成功获取首页HTML源码

<!--more-->

------

**一、代码分析**

闲话少叙，让我们开搞

```python
import requests
url = 'http://www.python-spider.com'
response = requests.get(url)
html = response.content.decode()
print(html)
```

​    直接发请求观察返回代码（目标网站**不限制**请求头、IP等进行，所以无需传入headers），查看返回代码----->

![微信图片_20200104211607](E:\www5\Hexo\BoyYongXin\source\_posts\images\微信图片_20200104211607.png)

{% asset_img E:\www5\Hexo\BoyYongXin\source\_posts\images\微信图片_20200104211607.png %}

​    没有错，就是上面一坨。**sojson.v5** 映入眼帘，这么恶心的代码，可以说毫无可读性。自然是要先js美化走一波（如何美化请自行百度 JS美化 第一个网页即可）

美化后，代码就清晰可见了------>

```javascript
;
var encode_version = 'sojson.v5',
    arktk = '__0x67e38',
    __0x67e38 = ['w7dkI2rDmcK0', 'HsK8E8K9wpk7DCEmew==', 'w4kwwr4qwrM=', 'w5czYMKJVkEowqg=', 'DBtxwq4=', 'wphiw6vCisKpcsOMwqTCh8OmLsKewobDg2PCgErCiRwQBwE9d8OVZcKbwpI=', 'McOXHUvDmB7ChHDCtQ==', 'JTvCosKMwoc=', '5Lm46IOe5YmJ6ZudO0tlw4nCsMK2ez0f', 'w4LDnFp+Ug==', 'UsKVwq7Co8OZw4w=', 'FXrDjsOEAFw=', 'wpvDksKeaSt8dg==', 'woB/wrslwqNkXhY='];
(function (_0x15e061, _0x24d8c2) {
    var _0x28d7df = function (_0x3a3784) {
        while (--_0x3a3784) {
            _0x15e061['push'](_0x15e061['shift']());
        }
    };
    _0x28d7df(++_0x24d8c2);
}(__0x67e38, 0x1c9));
var _0x421e = function (_0x6a3a69, _0x333514) {
    _0x6a3a69 = _0x6a3a69 - 0x0;
    var _0x39a8d7 = __0x67e38[_0x6a3a69];
    if (_0x421e['initialized'] === undefined) {
        (function () {
            var _0x22548a = typeof window !== 'undefined' ? window : typeof process === 'object' && typeof require === 'function' && typeof global === 'object' ? global : this;
            var _0x582a25 = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=';
            _0x22548a['atob'] || (_0x22548a['atob'] = function (_0x35d241) {
                var _0x5e5a32 = String(_0x35d241)['replace'](/=+$/, '');
                for (var _0x28a5e3 = 0x0, _0x2b45a3, _0x343cbf, _0xe0f1d0 = 0x0, _0x431162 = ''; _0x343cbf = _0x5e5a32['charAt'](_0xe0f1d0++);~ _0x343cbf && (_0x2b45a3 = _0x28a5e3 % 0x4 ? _0x2b45a3 * 0x40 + _0x343cbf : _0x343cbf, _0x28a5e3++ % 0x4) ? _0x431162 += String['fromCharCode'](0xff & _0x2b45a3 >> (-0x2 * _0x28a5e3 & 0x6)) : 0x0) {
                    _0x343cbf = _0x582a25['indexOf'](_0x343cbf);
                }
                return _0x431162;
            });
        }());
        var _0x5d2a8c = function (_0x354c7d, _0xac1c57) {
            var _0x3ef3da = [],
                _0x1e7821 = 0x0,
                _0x363fd2, _0x3b7037 = '',
                _0x3533d5 = '';
            _0x354c7d = atob(_0x354c7d);
            for (var _0x39c851 = 0x0, _0x63560e = _0x354c7d['length']; _0x39c851 < _0x63560e; _0x39c851++) {
                _0x3533d5 += '%' + ('00' + _0x354c7d['charCodeAt'](_0x39c851)['toString'](0x10))['slice'](-0x2);
            }
            _0x354c7d = decodeURIComponent(_0x3533d5);
            for (var _0xdb88b7 = 0x0; _0xdb88b7 < 0x100; _0xdb88b7++) {
                _0x3ef3da[_0xdb88b7] = _0xdb88b7;
            }
            for (_0xdb88b7 = 0x0; _0xdb88b7 < 0x100; _0xdb88b7++) {
                _0x1e7821 = (_0x1e7821 + _0x3ef3da[_0xdb88b7] + _0xac1c57['charCodeAt'](_0xdb88b7 % _0xac1c57['length'])) % 0x100;
                _0x363fd2 = _0x3ef3da[_0xdb88b7];
                _0x3ef3da[_0xdb88b7] = _0x3ef3da[_0x1e7821];
                _0x3ef3da[_0x1e7821] = _0x363fd2;
            }
            _0xdb88b7 = 0x0;
            _0x1e7821 = 0x0;
            for (var _0x3cdf31 = 0x0; _0x3cdf31 < _0x354c7d['length']; _0x3cdf31++) {
                _0xdb88b7 = (_0xdb88b7 + 0x1) % 0x100;
                _0x1e7821 = (_0x1e7821 + _0x3ef3da[_0xdb88b7]) % 0x100;
                _0x363fd2 = _0x3ef3da[_0xdb88b7];
                _0x3ef3da[_0xdb88b7] = _0x3ef3da[_0x1e7821];
                _0x3ef3da[_0x1e7821] = _0x363fd2;
                _0x3b7037 += String['fromCharCode'](_0x354c7d['charCodeAt'](_0x3cdf31) ^ _0x3ef3da[(_0x3ef3da[_0xdb88b7] + _0x3ef3da[_0x1e7821]) % 0x100]);
            }
            return _0x3b7037;
        };
        _0x421e['rc4'] = _0x5d2a8c;
        _0x421e['data'] = {};
        _0x421e['initialized'] = !![];
    }
    var _0x548bc2 = _0x421e['data'][_0x6a3a69];
    if (_0x548bc2 === undefined) {
        if (_0x421e['once'] === undefined) {
            _0x421e['once'] = !![];
        }
        _0x39a8d7 = _0x421e['rc4'](_0x39a8d7, _0x333514);
        _0x421e['data'][_0x6a3a69] = _0x39a8d7;
    } else {
        _0x39a8d7 = _0x548bc2;
    }
    return _0x39a8d7;
};
var timestamp = new Date()['valueOf']();
var token = window['btoa']('aiding_win' + String(Math[_0x421e('0x0', 'OoTU')](timestamp / 0xf4240)));
document[_0x421e('0x1', 'l[QL')] = _0x421e('0x2', '75NN') + token[_0x421e('0x3', 'xR0U')]('=', '') + 'sp' + token + _0x421e('0x4', 'vC8m');
document[_0x421e('0x5', 'n%hx')] = _0x421e('0x6', 'rDxU') + String(Math[_0x421e('0x7', 'vC8m')](timestamp / 0xf4240)) + _0x421e('0x8', '3B5*');
let nh = window['location'][_0x421e('0x9', 'L64&')];
let href = nh['split']('/');
if (href['length'] >= 0x3) {
    setTimeout('javascript:location.href=nh', 0x1f4);
} else {
    setTimeout(_0x421e('0xa', '&#k4'), 0x1f4);
}; if (!(typeof encode_version !== 'undefined' && encode_version === _0x421e('0xb', '5Khz'))) {
    window[_0x421e('0xc', '*#To')](_0x421e('0xd', '2e!l'));
};
encode_version = 'sojson.v5';
```

------

​    当看到这美化后的一坨代码的时候，我相信大家大家还是一脸懵逼。不过不要紧，让我们逐个分析

```javascript
var encode_version = 'sojson.v5',
    arktk = '__0x67e38',
    __0x67e38 = ['w7dkI2rDmcK0', 'HsK8E8K9wpk7DCEmew==', 'w4kwwr4qwrM=', 'w5czYMKJVkEowqg=', 'DBtxwq4=', 'wphiw6vCisKpcsOMwqTCh8OmLsKewobDg2PCgErCiRwQBwE9d8OVZcKbwpI=', 'McOXHUvDmB7ChHDCtQ==', 'JTvCosKMwoc=', '5Lm46IOe5YmJ6ZudO0tlw4nCsMK2ez0f', 'w4LDnFp+Ug==', 'UsKVwq7Co8OZw4w=', 'FXrDjsOEAFw=', 'wpvDksKeaSt8dg==', 'woB/wrslwqNkXhY='];
```

​    首先，声明了3个变量。暂时先放这就行了。

​    继续向下看，我们就会看到了一个匿名函数 ---> 

```javascript
(function (_0x15e061, _0x24d8c2) {
    var _0x28d7df = function (_0x3a3784) {
        while (--_0x3a3784) {
            _0x15e061['push'](_0x15e061['shift']());
        }
    };
    _0x28d7df(++_0x24d8c2);
}(__0x67e38, 0x1c9));
```

​    这个功能不难看出，是一个数组移位函数。我们继续向下看，就会看到一个很长很长的函数

```javascript
var _0x421e = function (_0x6a3a69, _0x333514) {
    _0x6a3a69 = _0x6a3a69 - 0x0;
    var _0x39a8d7 = __0x67e38[_0x6a3a69];
    if (_0x421e['initialized'] === undefined) {
        (function () {
            var _0x22548a = typeof window !== 'undefined' ? window : typeof process === 'object' && typeof require === 'function' && typeof global === 'object' ? global : this;
            var _0x582a25 = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=';
            _0x22548a['atob'] || (_0x22548a['atob'] = function (_0x35d241) {
                var _0x5e5a32 = String(_0x35d241)['replace'](/=+$/, '');
                for (var _0x28a5e3 = 0x0, _0x2b45a3, _0x343cbf, _0xe0f1d0 = 0x0, _0x431162 = ''; _0x343cbf = _0x5e5a32['charAt'](_0xe0f1d0++);~ _0x343cbf && (_0x2b45a3 = _0x28a5e3 % 0x4 ? _0x2b45a3 * 0x40 + _0x343cbf : _0x343cbf, _0x28a5e3++ % 0x4) ? _0x431162 += String['fromCharCode'](0xff & _0x2b45a3 >> (-0x2 * _0x28a5e3 & 0x6)) : 0x0) {
                    _0x343cbf = _0x582a25['indexOf'](_0x343cbf);
                }
                return _0x431162;
            });
        }());
        var _0x5d2a8c = function (_0x354c7d, _0xac1c57) {
            var _0x3ef3da = [],
                _0x1e7821 = 0x0,
                _0x363fd2, _0x3b7037 = '',
                _0x3533d5 = '';
            _0x354c7d = atob(_0x354c7d);
            for (var _0x39c851 = 0x0, _0x63560e = _0x354c7d['length']; _0x39c851 < _0x63560e; _0x39c851++) {
                _0x3533d5 += '%' + ('00' + _0x354c7d['charCodeAt'](_0x39c851)['toString'](0x10))['slice'](-0x2);
            }
            _0x354c7d = decodeURIComponent(_0x3533d5);
            for (var _0xdb88b7 = 0x0; _0xdb88b7 < 0x100; _0xdb88b7++) {
                _0x3ef3da[_0xdb88b7] = _0xdb88b7;
            }
            for (_0xdb88b7 = 0x0; _0xdb88b7 < 0x100; _0xdb88b7++) {
                _0x1e7821 = (_0x1e7821 + _0x3ef3da[_0xdb88b7] + _0xac1c57['charCodeAt'](_0xdb88b7 % _0xac1c57['length'])) % 0x100;
                _0x363fd2 = _0x3ef3da[_0xdb88b7];
                _0x3ef3da[_0xdb88b7] = _0x3ef3da[_0x1e7821];
                _0x3ef3da[_0x1e7821] = _0x363fd2;
            }
            _0xdb88b7 = 0x0;
            _0x1e7821 = 0x0;
            for (var _0x3cdf31 = 0x0; _0x3cdf31 < _0x354c7d['length']; _0x3cdf31++) {
                _0xdb88b7 = (_0xdb88b7 + 0x1) % 0x100;
                _0x1e7821 = (_0x1e7821 + _0x3ef3da[_0xdb88b7]) % 0x100;
                _0x363fd2 = _0x3ef3da[_0xdb88b7];
                _0x3ef3da[_0xdb88b7] = _0x3ef3da[_0x1e7821];
                _0x3ef3da[_0x1e7821] = _0x363fd2;
                _0x3b7037 += String['fromCharCode'](_0x354c7d['charCodeAt'](_0x3cdf31) ^ _0x3ef3da[(_0x3ef3da[_0xdb88b7] + _0x3ef3da[_0x1e7821]) % 0x100]);
            }
            return _0x3b7037;
        };
        _0x421e['rc4'] = _0x5d2a8c;
        _0x421e['data'] = {};
        _0x421e['initialized'] = !![];
    }
    var _0x548bc2 = _0x421e['data'][_0x6a3a69];
    if (_0x548bc2 === undefined) {
        if (_0x421e['once'] === undefined) {
            _0x421e['once'] = !![];
        }
        _0x39a8d7 = _0x421e['rc4'](_0x39a8d7, _0x333514);
        _0x421e['data'][_0x6a3a69] = _0x39a8d7;
    } else {
        _0x39a8d7 = _0x548bc2;
    }
    return _0x39a8d7;
};
```

​    这个函数比较复杂，乍一看看不出什么端倪，只能分析出这是一个函数（包含两个形参），并且有一个返回值，我们继续向下阅读剩余代码

```javascript
var timestamp = new Date()['valueOf']();
var token = window['btoa']('aiding_win' + String(Math[_0x421e('0x0', 'OoTU')](timestamp / 0xf4240)));
document[_0x421e('0x1', 'l[QL')] = _0x421e('0x2', '75NN') + token[_0x421e('0x3', 'xR0U')]('=', '') + 'sp' + token + _0x421e('0x4', 'vC8m');
document[_0x421e('0x5', 'n%hx')] = _0x421e('0x6', 'rDxU') + String(Math[_0x421e('0x7', 'vC8m')](timestamp / 0xf4240)) + _0x421e('0x8', '3B5*');
let nh = window['location'][_0x421e('0x9', 'L64&')];
let href = nh['split']('/');
if (href['length'] >= 0x3) {
    setTimeout('javascript:location.href=nh', 0x1f4);
} else {
    setTimeout(_0x421e('0xa', '&#k4'), 0x1f4);
}; if (!(typeof encode_version !== 'undefined' && encode_version === _0x421e('0xb', '5Khz'))) {
    window[_0x421e('0xc', '*#To')](_0x421e('0xd', '2e!l'));
};
encode_version = 'sojson.v5';
```

可以看出，这最后一部分的代码就很零散，隐隐约约的能看出一些关键字。但是想要理解代码含义还是有一定难度，最初笔者也卡住了很久，直到最后。。。我做了自己的网站，才看出这最后部分的端倪，我先上一段代码---->

```javascript
var timestamp = (new Date()).valueOf();
var token=window.btoa('aiding_win' + String(Math.round(timestamp/1000000)));
document.cookie='token=' + token.replace('=', '') + 'sp' + token+ '; path=/' ;
document.cookie='tokentime=' + String(Math.round(timestamp/1000000))+ '; path=/' ;
let nh = window.location.href;
let href = nh.split('/');
if (href.length >= 3) {
    setTimeout("javascript:location.href=nh", 500);
}
else {
    setTimeout("javascript:location.href='/'", 500);
}
```

​    上面的代码其实是我原始的cookie生成js代码。是不是发现了什么端倪？ 没错，最后一部分明显就是原始代码通过混淆得到的。所以我们就有了一个大胆的猜测

之前所有的代码实际上都是为了 _0x421e这个函数服务，最终的目的就是制造了一个真实的解密函数 _0x421e 来解密原始代码。既然有了猜测，我们就开始尝试破解它！

------

**二、反混淆**

其实反混淆的逻辑非常简单，通过第一部分的分析可知，整个sojson的加密是由四部分组合而成的

\1. 变量声明

\2. 数组移位

\3. 解密函数

\4. 执行函数

​    不说废话了，调试神奇 chrome console登场，新建标签页，F12，console一气呵成（有条件的可以使用 node.js，没有电脑的可以用口算/心算）。之后把前三部分丢进console里，怒敲回车！

{% asset_img E:\www5\Hexo\BoyYongXin\source\_posts\images\微信图片_20200104211316.png %}

![微信图片_20200104211316](E:\www5\Hexo\BoyYongXin\source\_posts\images\微信图片_20200104211316.png)

可以看到解密函数正常，并没有报错。那么整个反混淆就完成了。下面就是在解密函数的基础上断点调试找cookie实际生成代码，模拟后即可

```python
import requests
headers = {
"Accept":"application/json, text/javascript, */*; q=0.01",
"Accept-Encoding":"gzip, deflate",
"Accept-Language":"zh-CN,zh;q=0.9",
"Connection":"keep-alive",
"Cookie":"token=YWlkaW5nX3dpbjE1NzgxNDIspYWlkaW5nX3dpbjE1NzgxNDI=; tokentime=1578142",
"Host":"www.python-spider.com",
"Referer":"http://www.python-spider.com/content?sku=996",
"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36",
"X-Requested-With":"XMLHttpRequest",
}
url = 'http://www.python-spider.com/detail?skuID=996'
response = requests.get(url,headers=headers)
html = response.content.decode()
print(html)
```

代码里加入cookie生成方式：

补充一下知识点：

# Window btoa() 方法

------

## 定义和用法

btoa() 方法用于创建一个 base-64 编码的字符串。

该方法使用 "A-Z", "a-z", "0-9", "+", "/" 和 "=" 字符来编码字符串。

base-64 解码使用方法是 [atob()](https://www.runoob.com/jsref/met-win-atob.html) 。

## 语法

```
window.btoa(str)
```



最后python代码复现：

```python
# coding=utf-8
import base64
import time
import requests
headers = {
   
    "Cookie":"",
    "Host":"www.python-spider.com",
    "Referer":"http://www.python-spider.com/content?sku=996",
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36",
    "X-Requested-With":"XMLHttpRequest",
}

timestamp = time.time()*1000
result_str = 'aiding_win' + str(round(timestamp/1000000))
token = base64.encodebytes(result_str.encode('utf-8')).decode('utf-8')

cookie = 'token=%ssp%s'%(token.replace('=', ''),token)
tokentime ='tokentime=' + str(round(timestamp/1000000))
headers["Cookie"] = cookie + ";" + tokentime
headers["Cookie"] = headers["Cookie"].replace("\n","")
url = 'http://www.python-spider.com/detail?skuID=996'
response = requests.get(url,headers=headers)
html = response.content.decode()
print(html)
```

本文参考链接：http://www.python-spider.com/content?sku=996

https://blog.csdn.net/Mandyucan/article/details/80421711