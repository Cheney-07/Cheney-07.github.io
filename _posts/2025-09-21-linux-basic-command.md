---
layout:       post
title:        "linux基础命令"
author:       "Cheney"
header-style: text
catalog:      true
tags:
    - Linux
---

文件搜索和权限设置
chmod +x 文件	给文件加执行权限
chown 用户 文件	改变文件拥有者
cat access.log |grep ".php"
grep //文本搜索筛选
locate 文件名	快速查找文件（需先 sudo updatedb）
find 路径 -name "文件名"	根据名称找文件
管道符 |：组合多个命令，如 ps aux | grep nginx
通配符 *、?：*.txt 表示所有 .txt 文件

---------------------------
新建文件等命令
touch 文件名	创建空文件
cp 源 目标	复制文件/目录（-r 递归复制目录）
mv 源 目标	移动或重命名文件
rm 文件名	删除文件（-r 删除目录）(-f 强制删除)

----
查看任务进程
ss -tunlp	查看监听端口和进程
sleep 1//让当前shell暂停一秒钟
netstat -antp//查看网络连接状态
crontab -l//查看定时任务
ps aux//查看当前所有进程的状态
kill PID	//杀掉某个进程
traceroute 域名	路由追踪
dig 域名	DNS 查询（需安装）

----
下载资源等命令
curl URL	测试网页或 API 请求
wget URL	下载文件
sudo apt update	更新软件包索引
sudo apt upgrade	升级系统软件
sudo apt install 包名	安装软件
sudo apt remove 包名	卸载软件
dpkg -i 包.deb	安装 .deb 本地包

-------------------------------
设置服务命令
which 命令	查看命令路径
systemctl status 服务名	查看服务状态
systemctl start 服务名	启动服务
systemctl stop 服务名	停止服务
systemctl restart 服务名	重启服务