
# docker组件安装

标签（空格分隔）： docker

---
## 安装docker服务
&#160; &#160; &#160; &#160;初始操作系统是centos7，“uname -rs”显示内核如下
```
Linux 3.10.0-327.el7.x86_64
```
&#160; &#160; &#160; &#160;修改/etc/resolve.conf配置文件,添加如下内容
```
nameserver 114.114.114.114
nameserver 8.8.8.8
```
&#160; &#160; &#160; &#160;预安装工具包
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum makecache fast
```
&#160; &#160; &#160; &#160;安装docker-ce
```
sudo yum install docker-ce
```
&#160; &#160; &#160; &#160;启动docker服务
```
sudo systemctl start docker
```
&#160; &#160; &#160; &#160;安装docker镜像，选择ubuntu:16.04版本和centos:7.3.1611版本
```
docker pull ubuntu:16.04
docker pull centos:7.3.1611
```
&#160; &#160; &#160; &#160;常用命令
```
#显示所有镜像
docker images 
#显示所有运行的容器
docker ps -a 
#创建docker网络
docker network create --subnet=172.18.0.0/16 cloud_network 
#创建容器
## -p表示端口映射，12888为物理机段阔，2888为容器端口
## --privileged表示使用root权限创建容器
## -v表示挂载存储，/home/data/表示物理机存储，/usr/local/data/表示容器存储
## --name为docker容器定义名称
## --h为定义容器内系统host名称
## --net指定容器的网络
## --ip指定容器的ip地址
## --add-host在hosts文件中指定名称和地址的映射
## -it表示给容器分配一个终端方便访问
## centos:7.3.1611表示镜像名称
docker create -p 12888:2888 --privileged -v /home/data/:/usr/local/data/:ro --name cloud1 -h cloud1 --net=cloud_network --ip 172.18.0.2 --add-host master:192.168.137.129 -it centos:7.3.1611
#启动容器
docker start cloud1
#进入容器(此方法退出容器时，容器仍运行)
docker exec -it cloud1 centos:7.3.1611 /bin/bash
#创建并启动容器(此方法退出容器时，容器关闭)
docker run -it --privileged centos:7.3.1611 /bin/bash
#查看容器的ip地址,container_id需要修改为容器的id或名称
docker inspect container_id |grep IPAddress
docker inspect --format='{{.NetworkSettings.IPAddress}}' container_id
#提交容器并生成镜像(不推荐，但可用，推荐使用Dockfile创建镜像)
docker commit contain_id image_name
#删除容器,container_id需要修改为容器的id或名称
docker rm container_id
#删除镜像,image_id需要修改为镜像的id或名称
docker rmi image_id
#保存镜像,方便移植
docker save image_id > /usr/local/storage/image.tar
#载入镜像
docker load --input /usr/local/storage/image.tar

```
## 安装docker-compose
&#160; &#160; &#160; &#160;由于直接下载compose工具包安装失败，此处使用镜像安装docker-compose，版本选择1.14.0
```
docker pull docker/compose:1.14.0
curl -L --fail https://github.com/docker/compose/releases/download/1.14.0/run.sh > /usr/local/bin/docker-compose-install
chmod u+x docker-compose-install
cd /usr/local/bin/ && ./docker-compose-install
#测试是否安装成功
docker-compose
```
## 安装docker-machine
&#160; &#160; &#160; &#160;docker-machine无镜像，使用工具包安装
```
curl -L https://github.com/docker/machine/releases/download/v0.12.2/docker-machine-`uname -s`-`uname -m` >/tmp/dockermachine && chmod +x /tmp/docker-machine && sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```
