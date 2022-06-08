---
title: FastApi项目部署之应用生产环境gunicorn + uvicorn + nginx部署
date: 2022-06-07 14:47:02
categories: 后端
tags: [后端, fastapi]
---

hello 大家好我是Monday，今天给大家带来一篇fastApi项目部署的相关文章。

<!--more-->

在上篇介绍了使用Uvicorn部署启动程序

一般情况下，我们在开发、调试过程中采用命令行启动用的是 uvicorn（当然小型服务也有例外），但是并没有提供进程的监控。

所以我在生产环境下，一般会使用进程管理器 gunicorn + uvicorn + nginx 来部署项目



**1、Gunicorn：**

Gunicorn 是成熟的，功能齐全的服务器，Uvicorn 内部包含有 Guicorn 的 workers 类，允许你运行 ASGI 应用程序，这些 workers 继承了所有 Uvicorn 高性能的特点，并且给你使用 Guicorn 来进行进程管理。

这样的话，你可能动态增加或减少进程数量，平滑地重启工作进程，或者升级服务器而无需停机。

在生产环境中，Guicorn 大概是最简单的方式来管理 Uvicorn 了，生产环境部署我们推荐使用 Guicorn 和 Uvicorn 的 worker 类：

```text
gunicorn example:app -w 4 -k uvicorn.workers.UvicornWorker
```

**2、安装gunicorn**

```
pip install gunicorn
```

**3.以配置文件方式启动应用**

```python 
import multiprocessing
# 绑定ip和端口号
bind = '0.0.0.0:9088'
# 并行工作进程数
workers = multiprocessing.cpu_count() * 2 + 1
# workers = 2

# 还可以使用 gevent 模式，还可以使用sync模式，默认sync模式
worker_class = 'uvicron.workers.UvicornWorker'

# 指定每个工作者的线程数
threads = 1

# 监听队列
backlog = 2048

# 超过多少秒后工作将被杀掉，并重新启动。一般设置为30秒或更多
timeout = 30

# 设置最大并发量
worker_connections = 1000

# 默认False，设置守护进程，将进程交给supervisor管理
daemon = False

debug = True

loglevel = 'debug'

# 默认None，这会影响ps和top。如果要运行多个Gunicorn实例，
# 需要设置一个名称来区分，这就要安装setproctitle模块。如果未安装
proc_name = 'main'

# 设置进程文件目录
pidfile = './pid/gunicron.pid'

# 访问日志文件
accesslog = './logs/access.log'

# 错误日志文件
errorlog = './logs/error.log'
# logger_class = 'gunicron.gologging.Logger'

# 预加载资源
preload_app = True

autorestart = True

# 设置gunicron访问日志格式，错误日志无法设置
access_log_format = '%(t)s %(p)s %(h)s "%(r)s" %(s)s %(L)s %(b)s %(f)s" " "%(a)s"'


```

**4、启动程序**

```
nohup gunicorn -c gunicorn.conf.py main:app -k uvicorn.workers.UvicornWorker
```

**注意**：main.py的端口要和gunicorn绑定的端口一样。

```
uvicorn.run(app='main:app', host="127.0.0.1", port=9088, reload=True, debug=True)
```

**查看gunicorn进程树：**

```perl
pstree -ap|grep gunicorn
```

**杀掉进程：**

```
kill -9 gunicorn的pid
```

**5、配置nginx,** 

```
  vim /etc/nginx/conf.d/fastapi_9008.conf
```

**配置文件如下:**

```
server {
        listen 9008;
        root /python/fastapi;
        server_name xxx.xxx.xxx.xxx;
        location / {
            proxy_set_header x-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_pass http://localhost:9088/; # gunicorn绑定的端口号
        }
        # 配置static的静态文件：
        location ~ ^\/static\/.*$ {
            root /python/fastapi/static;
        }
}
```

**配置文件意思是**：

```
listen监听9008端口，

root指向项目目录，

server_name设定服务器IP或者域名，

location的proxy_set_header设定IP以及相关，

proxy_pass转发给gunicorn绑定的fastapi使用的端口，

注意：监听端口和转发绑定端口不能一样，
```

**然后保存，重启nginx**。

```
systemctl restart nginx
```



**nginx配置大文件上传**

正常web程序post是对请求的body或者文件上传没有大小限制

这个发布部署是通过nginx反向代理，转发fastapi端口，来实现的，

因为nginx默认最大上传文件是1M，所以需要修改，否则大文件会报错Request too large  413 代码，

把上面的nginx的配置文件，修改成如下：

```
server {
        listen 9518;
        root /www/python;
        server_name xxx.xxx.xxx.xxx;
        location / {
            proxy_set_header x-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_set_header Range $http_range;
            proxy_set_header If-Range $http_if_range;
            proxy_pass http://127.0.0.1:9088/; # gunicorn绑定的端口号
            client_max_body_size   2048m;#最大上传文件改成2G
            proxy_connect_timeout  3600s;#最大等待上传时间，改成1个小时
        }
}
```



**参考学习链接：**

nginx 更改配置client_max_body_size nginx.conf 修改默认限制上传附件大小

https://blog.csdn.net/z69183787/article/details/83070275

http请求的url或body或header有长度或大小的限制吗？

https://blog.csdn.net/kris_lh123/article/details/101062026

fastapi学习记录【十二】发布部署gunicorn+nginx

https://blog.csdn.net/wangluonanhai/article/details/124011178

FastAPI部署，docker 部署

https://blog.csdn.net/RoninYang/article/details/121128106

setproctitle：设置Python进程名称

https://www.missshi.cn/api/view/blog/5df835053b4ab21ff6000000

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./share/微信.png" style="zoom: 50%;" />