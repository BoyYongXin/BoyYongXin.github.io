---
title: AndroidStudio使用USB真机调试教程
date: 2021-06-07 22:34:00
categories: 逆向
tags: [逆向，AndroidStudio]
---

Android开发者第一步学习的应该就是真机调试了。但是很多初次接触android studio的同学还是不知道如何用真机调试，今天我就给大家写一个教程，希望可以帮到需要的人。

<!--more-->

## 一、真机调试

   1.先用usb线把你的测试手机连接到你的电脑上，并且安装驱动

   2.安装好驱动后就可以在电脑上读取手机的文件。接下来就是设置测试手机（寻找开发者选项）。

​    1）点击测试机“设置“，在设置中，寻找开发者选项，如果没有开发者选项，按接下来的步骤操作：找到关于手机，点击关于手机。然后找到“版本号”（小米手机是MIUI版本），点击几次版本号(最多5次)，系统即提示“您现在处于开发者模式”。注：不同手机提示可能不同。然后返回设置就会找到开发者选项。

 	2）然后在设置中找到“安全性（部分手机：系统安全）”，点击进入。找到“未知来源”，点击后会弹出系统提示，点击确认

​	 3）点击开发者选项，点击启用，会弹出一个系统提示，点击确认。然后再勾选USB调试，系统又会弹出提示，继续点击确认。

​	3.接下来设置android studio。打开android studio，在工具栏中找到，app选项，点击会弹出 Edit Configurations..选项，点击进入，然后在设置页面中找到  Deploymeng target Options下的Target选项，(新版可能找不到，可直接点击运行)，然后选择为USB Device。然后点击OK。至此咱就已经基本设置好了。可以进行真机测试了。

 结果如图所示：

<img src="./AndroidStudio使用USB真机调试教程/1.png" style="zoom: 50%;" />



## 二、Android应用各部分说明

**（1）MainActivity.java文件**
主要活动代码，实际的应用程序文件，将被转化为Dalvik可执行文件并运行。R.layout.activity_main 引用res/layout/activity_main.xml文件。

onCreate()  活动被加载之后众多被调用的方法之一。


**（2）AndroidManifest.xml文件**

AndroidManifest.xml文件是整个应用程序的信息描述文件，定义了应用程序中包含的Activity,Service,Content provider和BroadcastReceiver组件信息。每个应用程序在根目录下必须包含一个AndroidManifest.xml文件，且文件名不能修改。在AndroidManifest.xml文件中，首先看到是的<manifest>节点，它是整个应用程序的基本属性，涵盖了默认进程名字，应用程序标识，安装位置，对系统的要求以及应用程序的版本等。

android:icon是普通图标。

android:roundIcon是圆形图标。

android:label属性指定应用的名称。

android:name属性指定一个Activity类子类的全名。

意图过滤器的action被命名为android.intent.action.MAIN，表明这个活动被用做应用程序的入口。

意图过滤器的category被命名为android.intent.category.LAUNCHER，表明应用程序可以通过设备启动器的图标来启动。

@string指的是strings.xml，因此@string/app_name指的是定义在strings.xml中的app_name，这里实际为"demo"。


**（3）activity_main.xml文件**
activity_main.xml 我们将频繁修改这个文件来改变应用程序的布局。

TextView是一个用于构建用户图形界面的Android控件。

它包含有许多不同的属性，诸如android:layout_width, android:layout_height等用来设置它的宽度和高度等。

这里我们给它显示一句话“御风而上，神游天下!”，并且字体颜色为红色，引用自strings.xml文件。


## 三、建立一个空工程，编写获取序列号的简单例子

**1.获取手机状态需要设置权限**

```java
  package="com.example.demoimei">
    <uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
    <application
```

**2.编写布局文件**

```java
<TextView
    android:id="@+id/tv1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello World!" />

<TextView
    android:id="@+id/tv2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello World!" />
```

**3、编写主 Activity 类中的 onCreate 函数**

```java
package com.cockroach.hook_object;
import android.content.Context;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.telephony.TelephonyManager;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    
        TextView tv1 = (TextView) findViewById(R.id.tv1);
        TextView tv2 = (TextView) findViewById(R.id.tv2);
        TelephonyManager tm = (TelephonyManager)getSystemService(Context.TELEPHONY_SERVICE);
        tv1.setText("imei:" + tm.getDeviceId());
        tv2.setText("imsi:" + tm.getSubscriberId());
    }

}

```

如果出现闪退问题，报错

 java.lang.SecurityException: getDeviceId: Neither user 10226 nor current process has android.permis

这可能是手机设置的问题，我手机的 应用程序-> 授予权限就OK了

**运行结果如下：**

<img src="./AndroidStudio使用USB真机调试教程/2.jpg" style="zoom: 50%;" />



这就是我们的等会要hook的apk程序，下面编写xposed插件