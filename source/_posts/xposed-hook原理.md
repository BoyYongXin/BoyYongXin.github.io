---
title: xposed-hook原理
date: 2021-06-06 18:24:05
categories: 逆向
tags: [Hook, frida, 逆向]
---



xposed hook 原理介绍

<!--more-->



**1、xposed工作原理：**

Android基于Linux，第一个启动的进程自然是`init进程`，该进程会启动所有Android进程的父进程——`Zygote(孵化)进程`，该进程的启动配置在`/init.rc`脚本中，而Zygote进程对应的执行文件是`/system/bin/app_process`，该文件完成类库的加载以及一些函数的调用工作。在Zygote进程创建后，再fork出SystemServer进程和其他进程。而Xposed Framework呢，就是**用自己实现的app_process替换掉了系统原本提供的app_process**，加载一个额外的jar包，然后入口从原来的`com.android.internal.osZygoteInit.main()`被替换成了`de.robv.android.xposed.XposedBridge.main()`，然后**创建的Zygote进程就变成Hook的Zygote进程了**，而后面Fork出来的进程也是被Hook过的。这个Jar包在`/data/data/de.rbov.android.xposed.installer/bin/XposedBridge.jar`。



「拦截」。Xposed 通过劫持 Android 系统的 zygote 进程来加载自定义功能，这就像是半路截杀，在应用运行之前就已经将我们需要的自定义内容强加在了系统进程当中。

**2、Android 系统的Zygote初始化过程**

**参考链接：**https://www.jianshu.com/p/2303e7efb956

**3、xposed拦截流程：**

- startVM() 启动虚拟机并且初始化
- startReg() 注册一些JNI的方法
- start() 启动Zygote，依次执行 startVM()->startReg()->callMain()
- callMain() 通过反射调用传入的className对象的main函数，如在init.cpp函数里面传入的ZygoteInit对象和RuntimeInit对象，调用callMain方法就会调用对应的main方法。

**也就是说在安卓系统启动的时候，在执行callmain()这个函数的时候，我们就把我们的自定义的一些模块注入到，系统进程中了。**