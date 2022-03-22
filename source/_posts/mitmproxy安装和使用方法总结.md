---
title: mitmproxy安装和使用方法总结
date: 2022-03-09 17:08:15
categories: 技术杂谈
tags: [mitmproxy, 科学上网]
---

****

笔记_mitmproxy安装和使用方法总结



<!--more-->

**MitmProxy 介绍**：

支持 HTTP 和 HTTPS 的抓包程序，类似 Fiddler、Charles 的功能，只不过它是一个控制台的形式操作。
同时 MitmProxy 还有两个关联组件，一个是 MitmDump，它是 MitmProxy 的命令行接口，利用它我们可以对接 Python 脚本，用 Python 实现监听后的处理。另一个是 MitmWeb，它是一个 Web 程序，通过它我们可以清楚地观察到 MitmProxy 捕获的请求。


**相关链接**

mitmproxy不错的中文学习网站 https://blog.wolfogre.com/posts/usage-of-mitmproxy/ 

GitHub：https://github.com/mitmproxy/mitmproxy
官方网站：https://mitmproxy.org
PyPi：https://pypi.python.org/pypi/mitmproxy
官方文档：http://docs.mitmproxy.org
MitmDump脚本：http://docs.mitmproxy.org/en/stable/scripting/overview.html
下载地址：https://github.com/mitmproxy/mitmproxy/releases
DockerHub：https://hub.docker.com/r/mitmproxy/mitmproxy

报错可能解决方案： https://blog.csdn.net/andrew_wf/article/details/84991989 



**安装：**

 pip install mitmproxy 

 这是最简单和通用的安装方式，执行完毕之后即可完成 MitmProxy的安装，另外还安装了MitmDump、MitmWeb 两个组件 

**安装证书**

安装好mitmproxy后，到C:\Users\Administrator\.mitmproxy目录下

**电脑安装证书：**
mitmproxy-ca.p12

双击下一，直到最后

**手机安装证书：**
mitmproxy-ca.pem

**手机模拟器安装证书：**

mitmproxy-ca.pem

设置-安全-从SD卡导入证书即可



**这样我们就可以愉快的使用mitmproxy去搞一些事情了：**

**我下面用它抓取一下，皮皮虾app推荐模块抓取：**

首先建立两个py文件，一个用于app的视频json信息的抓取

**(1)get_request_data.py**  :

用来获取视频列表的url和headers

```python
import sys
sys.path.append('../')
import requests
import time
import mitmproxy
from mitmproxy import http
from mitmproxy import flow, proxy, controller, options
from mitmproxy.proxy.server import ProxyServer
import time
import re
'''
程序运行
 mitmdump -s xxxx.py
'''

def response(flow):

    if 'https://it.snssdk.com/bds/feed/stream/' in flow.request.url:
        datas = flow.request.headers
        headers = {}
        for k,v in datas.items():
            headers[k] = v
        print(f'root_url>>>{flow.request.url}<<<headers>>>{headers}<<<')


```

**(2)spider_data.py**  :

对获取到url和headers进行对视频信息的抓取

```Python
#coding=utf-8
'''
管道执行命令
 mitmdump -s <python1.y> | python <python2.py>
  mitmdump -s test.py | python receive.py
'''
from jsonpath import jsonpath
from glom import glom
import re
import sys
import requests
import json
for line in sys.stdin:
    root_url = re.search('root_url>>>(.*?)<<<', line)
    headers = re.search('headers>>>(.*?)<<<', line)
    if root_url and headers:
        html = requests.get(root_url.group(1),headers=eval(headers.group(1)))
        html.encoding = 'utf-8'
        data_info = html.json()

        data_list = glom(data_info, 'data.data')
        for datas in data_list:
            data = glom(datas, 'item.share')
            title = ''.join(jsonpath(data, "$..title"))
            url = ''.join(jsonpath(data, "$..compound_page_url"))
            image_url = ''.join(jsonpath(data, "$..image_url"))
            video_url = glom(datas, 'item.origin_video_download.url_list')[0].get('url')

            release_time = datas['item']['create_time']
            print(title)
            print(url)
            print(image_url)
            print(video_url)
            print(release_time)
            break

```

**怎么运行：**

进入cmd，窗口

mitmdump -s get_request_data.py | python spider_data.py



在手机端滑动就行了



**总结：**

中间抓取思路有很多种，get_request_data.py在文件里print()的信息写到文件里或数据库里，mitmdump -s get_request_data.py  在窗口执行这个命令，我们单独在写个程序，扫描数据库或文件，单独进行抓取数据



本文这个，只是小编想用一下，管道命令，



**结束语**：

 今天的分享就到这里了，欢迎大家关注微信公众号”**菜鸟童靴**“



