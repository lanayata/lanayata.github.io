
# docker下安装zookeeper集群

标签： docker zookeeper集群

---
## 初始环境
&#160; &#160; &#160; &#160;zookeeper集群安装在docker内，“docker --version” 显示版本为
```
Docker version 17.06.0-ce, build 02c1d87
```
&#160; &#160; &#160; &#160;docker镜像选择 `centos:7.3.1611`，镜像内核为
```
Linux 3.10.0-327.el7.x86_64
```
## 集群目标

|主机名称|IP地址|位置|
|:---|:---|:---|
|zookeeper0|172.18.2.1|master|
|zookeeper1|172.18.2.2|slave|
|zookeeper2|172.18.2.3|slave|

## 下载工具集
jdk选择：[jdk-8u131-linux-x64.tar.gz](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
zookeeper选择：[zookeeper-3.4.10.tar.gz](https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/)
## 创建zookeeper容器

1. 创建集群子网  
```
docker network create --subnet=172.18.0.0/16 cloud_network
```
2. 创建容器并只读模式挂载物理机/data目录到容器的/mnt目录，用于工具集文件的解压安装
```
docker create --privileged -v /data:/mnt:ro --name test -h test --net=cloud_network -it centos:7.3.1611
```
3. 启动容器
```
docker start test
```
4. 进入容器
```
docker exec -it test /bin/bash
```
5. 容器[root@test]内解压工具集
```
tar xvf /mnt/jdk-8u131-linux-x64.tar.gz -C /usr/local/ && mv /usr/local/jdk1.8.0_131 /usr/local/jdk
tar xvf /mnt/zookeeper-3.4.10.tar.gz -C /usr/local/ && mv /usr/local/zookeeper-3.4.10 /usr/local/zookeeper
```
6. vim编辑~/.bashrc文件配置环境变量
```
export JAVA_HOME=/usr/local/jdk
export JRE_HOME=/usr/local/jdk/jre
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$PATH
```
