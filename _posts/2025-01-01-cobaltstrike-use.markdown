---
layout:       post
title:        "服务端与客户端的配置"
author:       "Cheney"
header-style: text
catalog:      true
tags:
    - Web
    - JavaScript
---

首先将下载好的CS文件解压至linux系统(此处我以kali linux为例), 切换至CS文件夹目录打开终端, 输出如下命令用于搭建CS服务器

```bash
./teamserver 192.168.47.134 qq123456
```


java -XX:ParallelGCThreads=4 -XX:+AggressiveHeap -XX:+UseParallelGC -javaagent:uHook.jar -Dfile.encoding=utf-8 -jar cobaltstrike-client.jar


> 命令格式: `teamserver <服务端ip地址> <CS服务端密码>`
> 
> CS服务端的监听端口默认为50050, 若想更改监听端口可通过修改teamserver文件

![image-20220927215940294](https://img2023.cnblogs.com/other/3262985/202309/3262985-20230902004535017-1031220999.png)

切换至Windows虚拟机点击`start.bat`文件运行CS客户端界面, 并且输入: CS的服务器IP(192.168.47.134)、监听端口(50050)、自定义用户名、CS服务端密码(qq123456), 随后出现CS客户端界面
