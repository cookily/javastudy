# Docker安装官方Nexus3镜像构建Docker私服 实践笔记
---


**参考：[github官方文档](https://github.com/sonatype/docker-nexus3)**
-------

**我使用centos7X64最小化安装**
**CentOS-7-x86_64-Minimal-1708**

# 挂在github上的个人博客：[由hexo强力驱动 个人博客](http://code.cookily.cn/2018/06/14/Centos7%E4%B8%8B%E4%BD%BF%E7%94%A8Nexu%E6%9E%84%E5%BB%BADocker%E7%A7%81%E6%9C%8D/)

## 1.先获取nexus3镜像
```
docker pull sonatype/nexus3
```

## 2.创建给nexus3使用的数据目录
```
mkdir -p /nexus-data && chown -R 200 /nexus-data
```
## 3.运行nexus3镜像
```
docker run -d -p 8081:8081 --name nexus -v /nexus-data:/nexus-data --restart=always sonatype/nexus3
```
```
--restart=always 随docker启动而启动
```
**会需要一些时间，耐心等待...**

## 4.登录Nexus
**默认账号：admin**
**默认密码：admin123**
**我这边是虚拟机：所以地址是 虚拟机ip:8081**
**如果是阿里云：阿里云外网ip：8081，安全组规则里要开放8081**

## 5.创建docker仓库
**(1)点设置**
**(2)Repository>repositories>Create repository**
**(3)选择docker(hosted)**
**(4)设置仓库名字**
**(5)勾选http，填写端口(比如8888)**
**(6)保存**

## 6.加--insecure-registry参数
**解决push问题**
**先获取nexus3的id**
```
docker ps
```
**再获取nexus的ip**
```
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nexus3ID
```
```
vi /usr/lib/systemd/system/docker.service
```
**在ExecStart=/usr/bin/dockerd后面加入 --insecure-registry 上面获取的nexusIP:仓库端口**
```
ExecStart=/usr/bin/dockerd --insecure-registry 172.17.0.2:8888
```
**重启docker服务**
```
systemctl daemon-reload
systemctl restart docker
```

## 7.登录docker私服
```
docker login 172.17.0.2:8888
```
```
[root@docker ~]# docker login 172.17.0.2:8888
Username (admin): admin
Password: 
Login Succeeded
[root@docker ~]# 

```

## 8.打标记
**在上传镜像之前需要先打一个tag，用于版本标记。** 
**格式：**
**docker tag <imageId or imageName> <nexus-hostname>:<repository-port>/<image>:<tag>** 
**例如把官方nginx打标记：**
```
docker tag nginx 172.17.0.2:8888/chenyl/mynginx:v1
```
## 9.push上去
```
docker push 172.17.0.2:8888/chenyl/mynginx:v1
```
## 10.pull下来
```
docker pull 172.17.0.2:8888/chenyl/mynginx:v1
```

## 11.搜索私服镜像 
```
docker search 192.168.1.2:8082/chenyl
```
**192.168.1.2:8082/chenyl相关的镜像都查出来了**
```
[root@docker ~]# docker search 172.17.0.2:8888/chenyl/mynginx
NAME                                DESCRIPTION         STARS               OFFICIAL            AUTOMATED
172.17.0.2:8888/chenyl/mynginx:v1                       0                                       
172.17.0.2:8888/chenyl/mynginx:v2  
```