---
title: FastApi开发之项目文件结构
date: 2022-03-31 15:54:19
categories: 后端
tags: [后端, fastapi]
---

hello 大家好我是Monday，前面我已写了多篇关于fastapi的文章，我们可以开发一些web应用了，今天给大家带来一篇FastApi开发之项目文件结构的文章。

<!--more-->

根据自己的项目项目需求，组合了一套项目文件结构，今天的文章很短，主要是分享一下，每一个web应用的项目结构搭建

**1、项目结构如下：**

```
├─ceshi_server
│      active_send_request_ceshi.py
├─chain
│      scheduled_task.py
│      __init__.py
│
├─common
│  │  ceshi_db.py
│  │  MysqlSaveMethod.py
│  │  __init__.py
├─create_table
│      create_table.py
│      __init__.py
│
├─datas
│  ├─20220323
│  │      0e25fc005f6de92cfc618e6ff3d6d615.jpg
│  │      17416c4efa75596d835a39cced071b57.jpg
├─db
│  │  elastic_search_db.py
│  │  mongo_db.py
│  │  mysqldb.py
│  │  redis_db.py
│  │  __init__.py
├─extract_data
│  │  login.py
│  │  report_contact.py
│  │  report_new_msg.py
│  │  report_new_room.py
│  │  report_room_member_info.py
│  │  report_room_member_update.py
│  │  __init__.py
├─logs
│  └─loguru
│          2022-03-22.log
├─middleware
│  │  extract_data.py
│  │  limiter_tool.py
│  │  __init__.py
├─models
│  │  user.py
│  │  __init__.py
├─routers
│  │  index.py
│  │  open_api.py
│  │  pull_task.py
│  │  push_task.py
│  │  token_info.py
│  │  upload_file.py
│  │  __init__.py
├─schemas
│  │  user.py
│  │  __init__.py
│  
├─utils
│  │  loguru_handler.py
│  │  __init__.py
│  .gitignore
│  base_task.py
│  callback_server.py
│  config.py
│  crud.py
│  database.py
│  exceptions.py
│  readme.md
│  readme2.md
│  requirements.txt
│  start.sh
│  worker.py
│  __init__.py
```

**2、项目文件逐一介绍：**

ceshi_server文件：存放对结构的一些代码测试实例

chain和worker文件：是项目集成了，celery 处理一些后台任务脚本

DB文件：封装了一些mysql 、MongoDB、等数据基础增删改差的基本功能封装

common文件：做了一些公用的对DB数据库的常用方法封装

utils文件：封装一些常用的工具类，日志等

middleware：中间件模块，封装一些自定义中间件模块

logs文件： 存放日志落地文件

datas文件:存放数据缓存文件等

routers文件： 文件里各个应用模块的内容（主要是根据不同功能分类的分组路由）

models文件：存放数据模型

extract_data文件：因为的我项目，有对不同数数据的解析提取分类，故有此类模块的存在

schemas文件：对数据模型的合法校验

start.sh：项目启动脚本

exceptions：自定义异常类

 config文件：项目配置文件

callback_server:项目启动主文件

base_task.py： 里面主要封装了celery 的基类操作

 requirements.txt和readme.md这两个文件顾名思义，这里就不做介绍了



今天的文章分享内容，就到这了

**3、总结:**

陆陆续续写了关于fastapi 的开发的一些文章，今天的文章也预示着，关于fastapi框架的技术分享暂告一段落了，但并不是落幕

后续我还会对一些知识进行总结分享，感谢大家的观看

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./FastApi开发之项目文件结构/微信.png" style="zoom: 50%;" />











