---
title: SpringBoot集成Druid数据库连接池
date: 2022-05-24 17:15:21
categories: SpringBoot
tags: [SpringBoot,java]
---

hello 大家好我是Monday，今天我们开启SpringBoot的学习的系列文章之SpringBoot集成Druid数据库连接池。



<!--more-->

**什么是Druid**
Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，又不得不使用数据库连接池。

Druid 是阿里巴巴开源平台上一个数据库连接池实现，但它不仅仅是一个数据库连接池，它还包含一个ProxyDriver（代理驱动程序），一系列内置的JDBC组件库，一个SQL Parser(SQL解析器)。

Druid结合了 C3P0、DBCP 等 DB 池的优点，同时加入了日志监控，可以很好的监控 DB 池连接和 SQL 的执行情况，天生就是针对监控而生的 DB 连接池。（来源网络）

**导入maven依赖：**

```xml
<!--MySQL连接-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.39</version>
		</dependency>
		<!-- 连接池 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.31</version>
		</dependency>
		<!--dao层:mybatis-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.2.0</version>
		</dependency>
```

**设置Druid的配置**

```
############################################################
#
# mysql数据库配置
#
############################################################

spring.datasource.url = jdbc:mysql://localhost:3306/wechat  //数据库URL
spring.datasource.username = root   //账号
spring.datasource.password = 123456  //密码
spring.datasource.driverClassName = com.mysql.jdbc.Driver    // 驱动
## 指定使用druid 表明不使用默认的Hikari
spring.datasource.type= com.alibaba.druid.pool.DruidDataSource

## #############配置druid参数######################
#druid_config
#用户名
druid.login.username=root
#密码
druid.login.password=root

# 配置一个连接在池中最小生存的时间，单位是毫秒，下面是：5分钟
spring.datasource.druid.min-evictable-idle-time-millis= 300000
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.druid.pool-prepared-statements= true
spring.datasource.druid.max-pool-prepared-statement-per-connection-size=20
# 初始化大小，最小，最大
spring.datasource.druid.initial-size=5
spring.datasource.druid.min-idle= 3
# 最大连接池数量
spring.datasource.druid.max-active= 20
# 配置获取连接等待超时的时间
spring.datasource.druid.max-wait= 60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒，下面是：1分钟
spring.datasource.druid.time-between-eviction-runs-millis= 60000

# asyncInit是1.1.4中新增加的配置，如果有initialSize数量较多时，打开会加快应用启动时间
spring.datasource.druid.asyncInit=true

# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.druid.filters=stat,wall,log4j,config

spring.datasource.druid.validation-query: select 'x'
```

**配置Druid数据源监控**

**Druid 数据源具有监控的功能，并提供了一个 web 界面方便用户查看**

**1、配置 Druid 监控管理后台的Servlet**

```java
package com.example.demo.config;


import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import com.alibaba.druid.support.http.WebStatFilter;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;
import java.util.HashMap;

@Configuration
public class DruidConfig {
    @Value("${druid.login.username}")
    private String userName;

    @Value("${druid.login.password}")
    private String password;

    @Bean
    @ConfigurationProperties(prefix ="spring.datasource")//和配置文件绑定
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }
    //获取后台监控
    @Bean
    //由于 SpringBoot 默认是以 jar 包的方式启动嵌入式的 Servlet 容器来启动 SpringBoot 的 web 应用，没有 web.xml 文件。
    // 所以想用使用 Servlet 功能，就必须要借用 Spring Boot 提供的 ServletRegistrationBean 接口。
    public ServletRegistrationBean servletRegistration(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");   //StatViewServlet用于展示Druid的统计信息

        //设置后台登录的账户和密码
        HashMap<String, String> initParameters = new HashMap<>();

        //增加配置 登录的key是固定的
//        initParameters.put("loginUsername","admin");
//        initParameters.put("loginPassword","123456");
        initParameters.put("loginUsername",userName);
        initParameters.put("loginPassword",password);

        //设置谁可以访问
        initParameters.put("allow","");//任何人都可以访问

        //设置初始化参数
        bean.setInitParameters(initParameters);

        return bean;
    }

    //配置filter 过滤器
    //WebStatFilter：用于配置Web和Druid数据源之间的管理关联监控统计
    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
        filterRegistrationBean.setFilter(new WebStatFilter());
        filterRegistrationBean.addUrlPatterns("/*");
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return filterRegistrationBean;
    }

}

```

**2、启动SpringBoot测试，访问`http://localhost:8080/druid`**

网页登录用户名和密码就OK了



**参考链接：**

Spring Boot——集成Druid数据库连接池

https://blog.csdn.net/wpc2018/article/details/116948255

Druid使用手册

https://www.bookstack.cn/read/Druid/06014f428e7b0263.md

SpringBoot配置 Druid 连接池(application.properties参数配置详解

https://blog.csdn.net/yuekangwei/article/details/121369124

**结束语**：

​	今天的分享就到这里了，欢迎大家关注微信公众号"**菜鸟童靴**"

<img src="./share/微信.png" style="zoom: 50%;" />