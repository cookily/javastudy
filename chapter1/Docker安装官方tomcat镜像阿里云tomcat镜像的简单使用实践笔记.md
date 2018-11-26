# Docker安装官方tomcat 镜像 阿里云tomcat镜像 的简单使用  实践笔记
---



**参考：[docker tomcat 官方文档](https://hub.docker.com/_/tomcat/)**
-------


# 挂在github上的个人博客：[由hexo强力驱动 个人博客](http://code.cookily.cn/2018/06/14/Centos7%E4%B8%8Bdocket%E5%AE%89%E8%A3%85Tomcat%E5%92%8C%E4%BD%BF%E7%94%A8/)

## 1.首先查看下仓库镜像

```
docker search tomcat
```

```
[root@k8s~]# docker search tomcat
NAME                                  DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
tomcat                                Apache Tomcat is an open source implementati…   1710                [OK]                
dordoka/tomcat                        Ubuntu 14.04, Oracle JDK 8 and Tomcat 8 base…   47                                      [OK]
tomee                                 Apache TomEE is an all-Apache Java EE certif…   44                  [OK]                
davidcaste/alpine-tomcat              Apache Tomcat 7/8 using Oracle Java 7/8 with…   24                                      [OK]
consol/tomcat-7.0                     Tomcat 7.0.57, 8080, "admin/admin"              17                                      [OK]
cloudesire/tomcat                     Tomcat server, 6/7/8                            15                                      [OK]
bitnami/tomcat                        Bitnami Tomcat Docker Image                     10                                      [OK]
meirwa/spring-boot-tomcat-mysql-app   a sample spring-boot app using tomcat and My…   8                                       [OK]
tutum/tomcat                          Base docker image to run a Tomcat applicatio…   8                                       
jeanblanchard/tomcat                  Minimal Docker image with Apache Tomcat         8                                       
aallam/tomcat-mysql                   Debian, Oracle JDK, Tomcat & MySQL              6                                       [OK]
maluuba/tomcat7-java8                 Tomcat7 with java8.                             1                                       
99taxis/tomcat7                       Tomcat7                                         1                                       [OK]
primetoninc/tomcat                    Apache tomcat 8.5, 8.0, 7.0                     1                                       [OK]
picoded/tomcat7                       tomcat7 with jre8 and MANAGER_USER / MANAGER…   0                                       [OK]
oobsri/tomcat8                        Testing CI Jobs with different names.           0                                       
s390x/tomcat                          Apache Tomcat is an open source implementati…   0                                       
fabric8/tomcat-8                      Fabric8 Tomcat 8 Image                          0                                       [OK]
trollin/tomcat                                                                        0                                       
awscory/tomcat                        tomcat                                          0                                       
swisstopo/service-print-tomcat        backend tomcat for service-print "the true, …   0                                       
qminderapp/tomcat7                    Tomcat 7                                        0                                       
hegand/tomcat                         docker-tomcat                                   0                                       [OK]
buravelli9/tomcat-az-standards        Tomcat image-AZ                                 0                                       

```

## 2.然后从hub.docker.com或者第三方有加速的docker网站查看tomcat镜像版本

## [阿里云tomcat docker镜像连接](https://dev.aliyun.com/detail.html?spm=5176.1972343.2.2.2hEAlq&repoId=1270)
```
7.0.82-jre7, 7.0-jre7, 7-jre7, 7.0.82, 7.0, 7 (7/jre7/Dockerfile)
7.0.82-jre7-alpine, 7.0-jre7-alpine, 7-jre7-alpine, 7.0.82-alpine, 7.0-alpine, 7-alpine (7/jre7-alpine/Dockerfile)
7.0.82-jre8, 7.0-jre8, 7-jre8 (7/jre8/Dockerfile)
7.0.82-jre8-alpine, 7.0-jre8-alpine, 7-jre8-alpine (7/jre8-alpine/Dockerfile)
8.0.47-jre7, 8.0-jre7, 8.0.47, 8.0 (8.0/jre7/Dockerfile)
8.0.47-jre7-alpine, 8.0-jre7-alpine, 8.0.47-alpine, 8.0-alpine (8.0/jre7-alpine/Dockerfile)
8.0.47-jre8, 8.0-jre8 (8.0/jre8/Dockerfile)
8.0.47-jre8-alpine, 8.0-jre8-alpine (8.0/jre8-alpine/Dockerfile)
8.5.24-jre8, 8.5-jre8, 8-jre8, jre8, 8.5.24, 8.5, 8, latest (8.5/jre8/Dockerfile)
8.5.24-jre8-alpine, 8.5-jre8-alpine, 8-jre8-alpine, jre8-alpine, 8.5.24-alpine, 8.5-alpine, 8-alpine, alpine (8.5/jre8-alpine/Dockerfile)
9.0.2-jre8, 9.0-jre8, 9-jre8, 9.0.2, 9.0, 9 (9.0/jre8/Dockerfile)
9.0.2-jre8-alpine, 9.0-jre8-alpine, 9-jre8-alpine, 9.0.2-alpine, 9.0-alpine, 9-alpine (9.0/jre8-alpine/Dockerfile)
```

## 3.我选择8.5.24版本下载

```
docker pull tomcat:8.5.24
```

```
[root@k8s~]# docker pull tomcat:8.5.24
8.5.24: Pulling from library/tomcat
723254a2c089: Pull complete 
abe15a44e12f: Pull complete 
409a28e3cc3d: Pull complete 
a9511c68044a: Pull complete 
9d1b16e30bc8: Pull complete 
0fc5a09c9242: Pull complete 
d34976006493: Pull complete 
3b70003f0c10: Pull complete 
bc7887582e2e: Pull complete 
d2ab4f165865: Pull complete 
f671595c8b4b: Pull complete 
cc8cdb15e511: Pull complete 
Digest: sha256:e39e0a3982b6de106d95b4e06d6e94fdc5ec7d6cadf310f7fae02e4e182cd830
Status: Downloaded newer image for tomcat:8.5.24

```


## 4.查看本地镜像 可以看到已经在本地仓库

```
docker images
```

```
[root@k8s~]# docker images
REPOSITORY                                     TAG                 IMAGE ID            CREATED             SIZE
tomcat                                         8.5.24              66bbed06c8cd        2 weeks ago         557MB
```

## 5.开启tomcat一个容器(就是我们平常所说的实例)

```
docker run --name tomcat8.5.24 -d -v /etc/localtime:/etc/localtime:ro -p 8080:8080 tomcat:8.5.24
```

```
[root@k8s~]# docker run --name tomcat8.5.24 -d -v /etc/localtime:/etc/localtime:ro -p 8080:8080 tomcat:8.5.24
432bd371973ffb651083142f79913b675a8c400a7d011e7a5d3b4f70817f25b2

[root@k8s~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
432bd371973f        tomcat:8.5.24       "catalina.sh run"        56 seconds ago      Up 54 seconds       0.0.0.0:8080->8080/tcp   tomcat8.5.24
```
>-v /etc/localtime:/etc/localtime:ro 设置容器的时间与宿主机同步
>--name tomcat8.5.24 :添加备注名
>-p 8080:8080 :实体机的8080端口映射容器的8080
>docker ps :查看正在运行的容器

## 6.tomcat远程访问
**外网地址:8080**
**如果是阿里云esc服务器,安全组8080端口要开放**


## 7.如果是在同一个docker下的容器要连接tomcat
**7.1别名访问**
**比如tomcat2连接tomcat1,那tomcat2的启动参数里要加--link连接**
```
docker run -d -v /etc/localtime:/etc/localtime:ro --name tomcat1 tomcat:8.5.24
docker run -d -v /etc/localtime:/etc/localtime:ro --name tomcat2 --link tomcat1:tomcat1_随意别名 tomcat:8.5.24
```

```
在tomcat2容器里的tomcat连接地址就写为-->别名:8080
比如：http://192.168.123.123:8080
就写为:http://tomcat1_随意别名:8080
```
**7.2内网ip访问(相当于局域网访问)**
**获取tomcat容器内网ip地址**

```
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' tomcat8.5.24
```

```
[root@k8s~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
432bd371973f        tomcat:8.5.24       "catalina.sh run"        12 minutes ago      Up 12 minutes       0.0.0.0:8080->8080/tcp   tomcat8.5.24
1794aed7cfe1        mysql:5.6.38        "docker-entrypoint.s…"   3 hours ago         Up About an hour    0.0.0.0:3306->3306/tcp   mysql5.6.38
[root@k8s~]# docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' 1794aed7cfe1
172.17.0.3
```
>获取tomcat容器内网ip地址也可以使用管道过滤
```
docker inspect 1794aed7cfe1 | grep IPAddress
```
```
[docker@k8s~]# docker inspect 1794aed7cfe1|grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.3",
                    "IPAddress": "172.17.0.3"
```
```
在tomcat容器里的mysql连接地址就写为:内网ip
比如：jdbc:mysql://192.168.123.123:3306
jdbc:mysql://172.17.0.3:3306
```


## 8.进入tomcat容器

```
docker exec -it 容器名字或者id /bin/bash
```

```
[root@k8s~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
432bd371973f        tomcat:8.5.24       "catalina.sh run"        38 minutes ago      Up 38 minutes       0.0.0.0:8080->8080/tcp   tomcat8.5.24

[root@k8s~]# docker exec -it 432bd371973f /bin/bash
root@432bd371973f:/usr/local/tomcat# 

```

## 拿到shell,收工