---
title: FastApi学习之限制接口单位时间被访问频次
date: 2022-03-28 10:21:34
categories: 后端
tags: [后端, fastapi]
---

hello 大家好我是Monday，今天给大家带来一篇FastAPI中限制接口访问频次的文章。



<!--more-->

## 一、前言

​		对于服务端而言，有时候会碰到这么一个场景：某个接口需要在某个时间段内设置最高的访问次数来降低服务器的压力，比如之前用的某度的一些接口，一分钟内访问次数过高就会返回失败，等上个2分钟就又可以返回了。目的就是为了防止开发人员或者爬虫，甚至是恶意请求对服务器无限制的访问，降低服务器开支，因为一般的用户的请求是不会这么频繁的

## 二、实现方式

### **（1）Ratelimiter**

```
pip install ratelimiter
```

**python 中使用 Ratelimiter 来限制某方法的调用次数，用法如下**

```python
import time
from ratelimiter import RateLimiter

def limited(until):
    duration = int(round(until - time.time()))
    print('Rate limited, sleeping for {:d} seconds'.format(duration))
# 3秒之内只能访问2次
rate_limiter = RateLimiter(max_calls=2, period=3, callback=limited)

for i in range(3):
    with rate_limiter:
        print('Iteration', i)
```

**输出结果如下**

```javascript
Iteration 0
Iteration 1
Rate limited, sleeping for 3 seconds
Iteration 2
```

看到程序如期打印， callback 指定了超出指定次数是回调方法 

达到了预期的要求

### **（2）slowapi**

官网链接：[SlowApi Documentation](https://slowapi.readthedocs.io/en/latest/)

```
pip install slowapi
```

**fastapi中使用 slowapi 来限制某方法的调用次数，用法如下**

```python
import uvicorn
from fastapi import FastAPI
from slowapi import Limiter, _rate_limit_exceeded_handler
from fastapi import Request, Response
from slowapi.errors import RateLimitExceeded
from slowapi.util import get_remote_address
from fastapi.responses import PlainTextResponse

limiter = Limiter(key_func=get_remote_address)
app = FastAPI()
app.state.limiter = limiter
app.add_exception_handler(RateLimitExceeded, _rate_limit_exceeded_handler)


@app.get("/home")
@limiter.limit("5/minute")
async def homepage(request: Request):
    # return JSONResponse({"code":1})
    return PlainTextResponse("访问成功")


if __name__ == '__main__':
    uvicorn.run(app="ceshi:app", host="0.0.0.0", port=8000)

```

**测试代码如下：**

```python
import requests


headers = {
    "Connection": "keep-alive",
    "Cache-Control": "max-age=0",
    "sec-ch-ua": "^\\^",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "^\\^Windows^^",
    "Upgrade-Insecure-Requests": "1",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36 Edg/99.0.1150.52",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
    "Sec-Fetch-Site": "none",
    "Sec-Fetch-Mode": "navigate",
    "Sec-Fetch-User": "?1",
    "Sec-Fetch-Dest": "document",
    "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6"
}
url = "http://localhost:8000/home"
for i in range(10):
    response = requests.get(url, headers=headers)
    print(response.text)
```

**测试结果：**

```
test
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}
{"error":"Rate limit exceeded: 5 per 1 minute"}

```

### （3）walrus

```
pip install walrus
```

**fastapi中使用 walrus 来限制某方法的调用次数，用法如下**

```python
from walrus import Database, RateLimitException
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
import uvicorn

db = Database()
rate = db.rate_limit('xxx', limit=5, per=60)  # in 60s just can only click 5 times

app = FastAPI()


@app.exception_handler(RateLimitException)
def parse_rate_litmit_exception(request: Request, exc: RateLimitException):
    msg = {'success': False, 'msg': f'please have a tea for sleep, your ip is: {request.client.host}.'}
    return JSONResponse(status_code=429, content=msg)


@app.get('/')
@rate.rate_limited(lambda request: request.client.host)
def index(request: Request):
    return {'success': True}


@app.get('/important_api')
@rate.rate_limited(lambda request: request.client.host)
def query_important_data(request: Request):
    data = 'important data'
    return {'success': True, 'data': data}


if __name__ == "__main__":
    uvicorn.run("ceshi2:app", debug=True, reload=True)

```



**项目完整代码：**

[BoyYongXin/wx_pub_article_code: 博客发文使用的代码 (github.com)](https://github.com/BoyYongXin/wx_pub_artcole_code)

参考博客：

[反爬虫策略手把手教你使用FastAPI来限制接口的访问速率 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1769464)

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./FastApi学习之限制接口单位时间被访问频次/微信.png" style="zoom: 50%;" />