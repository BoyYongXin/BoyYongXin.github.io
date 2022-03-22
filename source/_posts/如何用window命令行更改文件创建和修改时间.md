---
title: 如何用window命令行更改文件创建和修改时间
date: 2020-10-17 15:49:35
categories: 技术杂谈


---

昨天朋友突然给我发来消息，说他有个文件，文件的创建时间和修改时间，想修改一下，往前提个几天，刚开始给他推荐了几个软件，但是想想一个程序员怎么能用他人的软件呢，于是乎，安排........

 <!--more-->

### 1. 步骤：

　　新建一个bat文件，在其中添加语句：

```
@ECHO OFF
powershell.exe -command "ls 'folder_path\*.dll' | foreach-object { $_.LastWriteTime = Get-Date; $_.CreationTime = Get-Date; $_.LastAccessTime = Get-Date;}"
PAUSE
```

 

### 2. 解释：

　　代码将folder_path路径下的所有dll文件的创建时间和修改时间改成现在的时间。

　　-command: tells powershell to run the following command and return immediately

　　ls: list all matching files at the path specified

　　foreach-object: run the following block on each file that ls found

　　$_.LastWriteTime = Get-Date: for each file, set the LastWriteTime to the value returned by Get-Date (today’s date and time)

　　$_.CreationTime = Get-Date： for each file, set the CreationTime to the value returned by Get-Date (today’s date and time)

###  3. 修改至指定时间：

　　将.LastWriteTime=Get−Date:改为.LastWriteTime=Get−Date:改为_.LastWriteTime = '01/11/2004 22:13:36'：

```
@ECHO OFF
powershell.exe -command "ls 'folder_path\*.*' | foreach-object { $_.LastWriteTime = '01/11/2004 22:13:36'; $_.CreationTime = '01/11/2004 22:13:36' }"
PAUSE
```

### 4. 递归文件夹中所有文件：

```
@ECHO OFF
powershell.exe -command "Get-Childitem -path 'E:\project_llj\install\test\' -Recurse | foreach-object { $_.LastWriteTime = Get-Date; $_.CreationTime = Get-Date }" 
PAUSE
```

