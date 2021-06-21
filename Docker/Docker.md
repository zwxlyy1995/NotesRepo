# Docker 笔记

***
- [Docker 笔记](#docker-笔记)
  - [Docker 安装](#docker-安装)
  - [Docker 使用](#docker-使用)
    - [1. 容器使用](#1-容器使用)

***

## Docker 安装

***

## Docker 使用

### 1. 容器使用

1. 获取镜像
```
$ docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```
OPTIONS:
+ -a : 拉取所有tagged镜像
+ --disable-content-trust :忽略镜像的校验,默认开启

2. 启动容器
```
$ docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```
OPTIONS:
+ -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
+ -d: 后台运行容器，并返回容器ID；
+ -i: 以交互模式运行容器，通常与 -t 同时使用；
+ -P: 随机端口映射，容器内部端口随机映射到主机的端口
+ -p: 指定端口映射，格式为：主机(宿主)端口:容器端口
+ -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
+ --name="nginx-lb": 为容器指定一个名称；
+ --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
+ --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
+ -h "mars": 指定容器的hostname；
+ -e username="ritchie": 设置环境变量；
+ --env-file=[]: 从指定文件读入环境变量；
+ --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
+ -m :设置容器使用内存最大值；
+ --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
+ --link=[]: 添加链接到另一个容器；
+ --expose=[]: 开放一个端口或一组端口；
+ --volume , -v: 绑定一个卷




***

本文主要参考自：

<p align="right">zwxupc@163.com 2020.10.10</p>