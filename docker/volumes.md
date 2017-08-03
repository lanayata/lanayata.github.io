
# docker创建数据卷

标签：docker 数据卷

---
&#160; &#160; &#160; &#160;镜像选择centos，版本号是7.3.1611，数据卷名称volumes，默认以读写的方式将物理机的/home/data目录映射到容器的/usr/local/storage目录
```
#创建数据卷
sudo docker create -v /home/data:/usr/local/storage --name volumes centos:7.3.1611
#启动数据卷
docker start volumes
```
&#160; &#160; &#160; &#160;挂载数据卷，测试查看/usr/local/storage目录是否存在
```
sudo docker run -it --volumes-from volumes --name test centos:7.3.1611
```
