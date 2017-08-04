
# 基于Dockerfile创建镜像
&#160; &#160; &#160; &#160;镜像文件Dockerfile的内容如下（样本文件）
```
#this dockerfile use the centos image
#version 7.3.1611
#Author: jun.peng@youbiai.com

#指定基础镜像
FROM centos:7.3.1611

#维护者信息
MAINTAINER jun.peng jun.peng@youbiai.com

#挂在镜像映射目录
VOLUME /usr/local/storage

#对外开放端口号
EXPOSE 80
EXPOSE 443

#定义变量
ENV DEST_STORAGE /usr/local/storage

#复制物理机当前Dockerfile目录内的abc文件到容器的$DEST_STORAGE/目录下
COPY abc $DEST_STORAGE/

#指定容器启动时的工作目录
WORKDIR $DEST_STORAGE

#镜像启动时的操作指令
RUN uname -rs > $DEST_STORAGE/test

#镜像启动时执行指令，如果指令较多使用脚本，否则只执行最后一条
CMD cat $DEST_STORAGE/test
```
&#160; &#160; &#160; &#160;创建镜像(使用当前目录下的Dockerfile创建)
```
docker build -t image_name:image_version .
```
