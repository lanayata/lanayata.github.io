
# docker下安装zookeeper集群

标签： docker zookeeper集群

---
## 初始环境
&#160; &#160; &#160; &#160;zookeeper集群安装在docker内，“docker --version” 显示版本为`Docker version 17.06.0-ce, build 02c1d87`，docker镜像选择 `centos:7.3.1611`，镜像内核`Linux 3.10.0-327.el7.x86_64`

## 集群目标

|主机名称|IP地址|位置|
|:-|:-|:-|:-|
|zookeeper0|172.18.2.1|master|
|zookeeper1|172.18.2.2|slave|
|zookeeper2|172.18.2.3|slave|
## 下载工具集

jdk选择：[`jdk-8u131-linux-x64.tar.gz`](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
zookeeper选择：[`zookeeper-3.4.10.tar.gz`](https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/)
## 未完待续
