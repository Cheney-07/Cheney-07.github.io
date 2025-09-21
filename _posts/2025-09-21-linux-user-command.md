---
layout:       post
title:        "linux基础命令"
author:       "Cheney"
header-style: text
catalog:      true
tags:
    - Linux
---

 ssh w1r3s@192.168.233.136//ssh远程连接命令，ssh 用户名 @ip地址
## 一. uname
**1、uname -r ： 显示操作系统的发行版号**  
**2、uname -a ：显示系统名、节点名称、操作系统的发行版号、内核版本等等。**

## 二. id
Linux id命令用于显示用户的ID，以及所属群组的ID。

id 会显示用户以及所属群组的实际与有效 ID，若两个 ID 相同，则仅显示实际 ID，若仅指定用户名称，则显示目前用户的 ID。

该命令会显示用户的 UID（User ID）、GID（Group ID）以及附属于用户的所有组 ID。
### 语法

id \[-gGnru\]\[--help\]\[--version\]\[用户名称\]

**参数说明**：
- \-g 或 --group 　显示用户所属群组的ID。
- \-G 或 --groups 　显示用户所属附加群组的ID。
- \-n 或 --name 　显示用户，所属群组或附加群组的名称。
- \-r 或 --real 　显示实际ID。
- \-u 或 --user 　显示用户ID。
- \-help 　显示帮助。
- \-version 　显示版本信息。
### 实例

显示当前用户信息：

 id //显示当前用户ID

显示用户群组的 ID：
id -g

显示所有群组的 ID：
d -G

显示指定用户信息
 id hnlinux

三.sudo
sudo -u [目标用户 ]     [要执行的命令]
功能：切换到目标用户执行命令

sudo -l
功能：列出当前用户在 sudo 下被授权可以执行哪些命令。
User w1r3s may run the following commands on W1R3S.localdomain:
    (ALL : ALL) ALL
User w1r3s
→ 说明当前用户名是 w1r3s。
(ALL : ALL) ALL
→ 这是 sudoers 文件中的权限表达式，意思是：
ALL（第一个）：表示可以以任何用户身份运行命令（即可以 sudo -u 成任何用户，包括 root）。
ALL（第二个）：表示可以以任何组身份运行命令（可以用 -g 切换组）。
最后的 ALL：表示可以运行系统上的所有命令，没有限制。

--------------------------------------------------------------------------

|   |   |   |
|---|---|---|
|`sudo -i`|模拟完整 root 登录环境|获得 root shell，加载 root 的 profile/env|
|`sudo su`|切换到 root 用户|获得 root shell，但环境继承略不同|

## 一、`sudo -i` 

`sudo --login`

- 等效于以 root 身份重新登录一遍系统；
    
- 会读取 root 用户的：
    
    - `/root/.profile`
        
    - `/root/.bashrc`
        
    - `/etc/profile`
        
    - `/etc/environment`
        
- 环境变量、PATH、UMASK 都是 root 的。
    
**模拟一个全新 root 登录环境。**
sudo -i 
whoami  root 
pwd     /root 
env     全 root 环境`

---

## 二、`sudo su` 

`sudo (以默认 root 身份执行) su (切换到 root 用户)`

- `sudo` 只管授权，实际 shell 还是 `su` 负责切换；
    
- 环境变量大多数继承自当前用户；
    
- 不一定会完全读取 root 的 profile；
    
- 你的 PATH 可能还是你自己用户的 PATH；
