---
title: fastApi项目部署之uvicorn参数解析
date: 2022-06-07 13:57:43
categories: 后端
tags: [后端, fastapi]
---

hello 大家好我是Monday，今天给大家带来一篇fastApi项目部署的相关文章。

<!--more-->



## Uvicorn

uvicorn官方文档：https://www.uvicorn.org/

- `Uvicorn` 是基于 `uvloop` 和 `httptools` 构建的非常快速的 `ASGI` 服务器。
- `Uvicorn` 提供一个轻量级的方法来运行多个工作进程，比如 `-workers 4` ，但是并没有提供进程的监控。



**1、uvicorn支持的参数非常多 uvicorn --help  查看下：**

```
uvicorn --help
Usage: uvicorn [OPTIONS] APP

Options:
  --host TEXT                     Bind socket to this host.  [default:
                                  127.0.0.1]

  --port INTEGER                  Bind socket to this port.  [default: 8000]
  --uds TEXT                      Bind to a UNIX domain socket.
  --fd INTEGER                    Bind to socket from this file descriptor.
  --reload                        Enable auto-reload.
  --reload-dir TEXT               Set reload directories explicitly, instead
                                  of using the current working directory.

  --reload-delay FLOAT            Delay between previous and next check if
                                  application needs to be. Defaults to 0.25s.
                                  [default: 0.25]

  --workers INTEGER               Number of worker processes. Defaults to the
                                  $WEB_CONCURRENCY environment variable if
                                  available. Not valid with --reload.

  --loop [auto|asyncio|uvloop]    Event loop implementation.  [default: auto]
  --http [auto|h11|httptools]     HTTP protocol implementation.  [default:
                                  auto]

  --ws [auto|none|websockets|wsproto]
                                  WebSocket protocol implementation.
                                  [default: auto]

  --lifespan [auto|on|off]        Lifespan implementation.  [default: auto]
  --interface [auto|asgi3|asgi2|wsgi]
                                  Select ASGI3, ASGI2, or WSGI as the
                                  application interface.  [default: auto]

  --env-file PATH                 Environment configuration file.
  --log-config PATH               Logging configuration file.
  --log-level [critical|error|warning|info|debug|trace]
                                  Log level. [default: info]
  --access-log / --no-access-log  Enable/Disable access log.
  --use-colors / --no-use-colors  Enable/Disable colorized logging.
  --proxy-headers / --no-proxy-headers
                                  Enable/Disable X-Forwarded-Proto,
                                  X-Forwarded-For, X-Forwarded-Port to
                                  populate remote address info.

  --forwarded-allow-ips TEXT      Comma seperated list of IPs to trust with
                                  proxy headers. Defaults to the
                                  $FORWARDED_ALLOW_IPS environment variable if
                                  available, or '127.0.0.1'.

  --root-path TEXT                Set the ASGI 'root_path' for applications
                                  submounted below a given URL path.

  --limit-concurrency INTEGER     Maximum number of concurrent connections or
                                  tasks to allow, before issuing HTTP 503
                                  responses.

  --backlog INTEGER               Maximum number of connections to hold in
                                  backlog

  --limit-max-requests INTEGER    Maximum number of requests to service before
                                  terminating the process.

  --timeout-keep-alive INTEGER    Close Keep-Alive connections if no new data
                                  is received within this timeout.  [default:
                                  5]

  --ssl-keyfile TEXT              SSL key file
  --ssl-certfile TEXT             SSL certificate file
  --ssl-keyfile-password TEXT     SSL keyfile password
  --ssl-version INTEGER           SSL version to use (see stdlib ssl module's)
                                  [default: 2]

  --ssl-cert-reqs INTEGER         Whether client certificate is required (see
                                  stdlib ssl module's)  [default: 0]

  --ssl-ca-certs TEXT             CA certificates file
  --ssl-ciphers TEXT              Ciphers to use (see stdlib ssl module's)
                                  [default: TLSv1]

  --header TEXT                   Specify custom default HTTP response headers
                                  as a Name:Value pair

  --version                       Display the uvicorn version and exit.
  --app-dir TEXT                  Look for APP in the specified directory, by
                                  adding this to the PYTHONPATH. Defaults to
                                  the current working directory.  [default: .]

  --help                          Show this message and exit.
```

**2、我们进入到 uvicorn.run 查看下参数配置，及默认值**

```
class Config:
    def __init__(
        self,
        app,
        host="127.0.0.1",
        port=8000,
        uds=None,
        fd=None,
        loop="auto",
        http="auto",
        ws="auto",
        lifespan="auto",
        env_file=None,
        log_config=LOGGING_CONFIG,
        log_level=None,
        access_log=True,
        use_colors=None,
        interface="auto",
        debug=False,
        reload=False,
        reload_dirs=None,
        reload_delay=None,
        workers=None,
        proxy_headers=True,
        forwarded_allow_ips=None,
        root_path="",
        limit_concurrency=None,
        limit_max_requests=None,
        backlog=2048,
        timeout_keep_alive=5,
        timeout_notify=30,
        callback_notify=None,
        ssl_keyfile=None,
        ssl_certfile=None,
        ssl_keyfile_password=None,
        ssl_version=SSL_PROTOCOL_VERSION,
        ssl_cert_reqs=ssl.CERT_NONE,
        ssl_ca_certs=None,
        ssl_ciphers="TLSv1",
        headers=None,
    ):
```

**3、 解析每一个参数的含义**

```
app：指定应用app，'脚本名:FastAPI实例对象'、FastAPI实例对象


host: 字符串，允许被访问的形式 locahost、127.0.0.1、当前IP、0.0.0.0，默认为127.0.0.1,


port：数字，应用的端口，默认为8000,


uds：字符串，socket服务绑定到UNIX的域名


fd：数字，从此文件描述符绑定到socket


loop：[auto|asyncio|uvloop]，事件循环模式，默认为auto


http：[auto|h11|httptools]，HTTP协议实现，默认为auto


ws：[auto|none|websockets|wsproto]，WebSocket协议实现，默认为auto


ws-max-size：数字，WebSocket最大消息大小（字节），默认值为16777216


lifespan：[auto|on|off]，生命周期实施，默认为auto


env-file：PATH，环境配置文件


log-config：PATH，日志配置文件。支持的格式：.ini、.json、.yaml，默认为fastapi默认的log配置


log-level：[critical|error|warning|info|debug|trace]，日志级别，默认info


access-log：boolean，access log日志的开关，默认为True


use-colors：boolean，彩色日志的开关，（前提需指定log-config），默认为None


interface：[auto|asgi3|asgi2|wsgi]，选择ASGI3、ASGI2或WSGI作为应用程序接口，默认为auto


debug：是否使用debug模式，默认False,


reload：boolean，当代码发生更时，是否自动重启，默认False,


reload_dirs：字符串，设置重新加载目录，由源码可见，当没有传这个参数的实时，将取当前工作目录


reload-delay：float，每隔多久检测代码是否有变动，默认0.25秒


workers：数字，工作进程数。默认为$WEB\U CONCURRENCY环境变量（如果可用），或1。对于--reload无效。


proxy-headers：boolean，启用/禁用X-Forwarded-Proto、X-Forwarded-For、X-Forwarded-Port以填充远程地址信息，默认为True


forwarded-allow-ips：字符串，用逗号分隔的IP列表以信任代理标头。默认为$FORWARDED\u ALLOW\u IPS环境变量（如果可用），或 None，为None时，代码里面则取127.0.0.1


root-path：字符串，为安装在给定URL路径下的应用程序设置ASGI“根路径”。


limit-concurrency：数字，在发出HTTP503响应之前，允许的最大并发连接数或任务数。默认为None


limit-max-requests：数字，达到多少请求数则终止进程，默认为None


backlog：数字，等待处理的最大连接数，默认为2048


timeout-keep-alive：数字，如果在此超时时间内未收到新数据，则关闭保持活动状态的连接，默认为5


ssl-keyfile：字符串，SSL密钥文件，默认为None


ssl-certfile：字符串，SSL证书文件，默认为None


ssl-keyfile-password：字符串，SSL密钥文件密码，默认为None


ssl-version：数字，要使用的SSL版本（详见stdlib SSL模块），默认为2


ssl-cert-reqs：数字，是否需要客户端证书（详见stdlib SSL模块），默认为0


ssl-ca-certs：字符串，CA证书文件


ssl-ciphers：字符串，要使用的CA证书文件密码（详见stdlib SSL模块），默认为TLSv1


header：字典，自定义响应头信息，键值对的形式，默认为None

```



**4、利用uvicorn 部署启动FastApi应用程序**

```python
import uvicorn
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()  # 必须实例化该类，启动的时候调用


class People(BaseModel):  # 必须继承
    name: str
    age: int
    address: str
    salary: float


# 请求根目录
@app.get('/')
def index():
    return {'message': '欢迎来到FastApi 服务！'}


# get请求带参数数据
@app.get('/items/{item_id}')
def items(item_id: int):
    return {'message': '欢迎' + item_id + '来到接口页面'}


# post请求带参数数据
@app.post('/people')
def insert(people: People):
    age = people.age
    msg = f'名字：{people.name}，年龄：{age}'
    return {'success': True, 'msg': msg}


if __name__ == '__main__':
    uvicorn.run(app="ceshi:app", host="127.0.0.1", port=8080, workers=10, 
                debug=True, reload=True)

```

**5、启动：**

```
1、python ceshi.py

2、uvicorn main:app --reload
```

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./share/微信.png" style="zoom: 50%;" />