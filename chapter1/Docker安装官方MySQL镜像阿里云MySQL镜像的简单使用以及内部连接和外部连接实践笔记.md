# Docker安装官方MySQL 镜像 阿里云MySQL 镜像 的简单使用以及内部连接和外部连接 实践笔记
---


**参考：[阿里mysql docker 文档](https://dev.aliyun.com/detail.html?spm=5176.1972343.2.2.79825aaaVNcsUP&repoId=1239)**

## 1.首先查看下仓库镜像

```
docker search mysql
```

```
[root@iZ0p61por0uylmZ ~]# docker search mysql
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                                                  MySQL is a widely used, open-source relation…   5676                [OK]                
mariadb                                                MariaDB is a community-developed fork of MyS…   1760                [OK]                
mysql/mysql-server                                     Optimized MySQL Server Docker images. Create…   390                                     [OK]
zabbix/zabbix-server-mysql                             Zabbix Server with MySQL database support       78                                      [OK]
hypriot/rpi-mysql                                      RPi-compatible Docker Image with Mysql          78                                      
centurylink/mysql                                      Image containing mysql. Optimized to be link…   58                                      [OK]
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   41                                      [OK]
tutum/mysql                                            Base docker image to run a MySQL database se…   31                                      
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          25                                      [OK]
mysql/mysql-cluster                                    Experimental MySQL Cluster Docker images. Cr…   19                                      
centos/mysql-57-centos7                                MySQL 5.7 SQL database server                   17                                      
schickling/mysql-backup-s3                             Backup MySQL to S3 (supports periodic backup…   16                                      [OK]
linuxserver/mysql                                      A Mysql container, brought to you by LinuxSe…   14                                      
zabbix/zabbix-proxy-mysql                              Zabbix proxy with MySQL database support        10                                      [OK]
centos/mysql-56-centos7                                MySQL 5.6 SQL database server                   7                                       
openshift/mysql-55-centos7                             DEPRECATED: A Centos7 based MySQL v5.5 image…   6                                       
circleci/mysql                                         MySQL is a widely used, open-source relation…   3                                       
frodenas/mysql                                         A Docker Image for MySQL                        3                                       [OK]
dsteinkopf/backup-all-mysql                            backup all DBs in a mysql server                3                                       [OK]
inferlink/landmark-mysql                               landmark-mysql                                  0                                       [OK]
cloudposse/mysql                                       Improved `mysql` service with support for `m…   0                                       [OK]
cloudfoundry/cf-mysql-ci                               Image used in CI of cf-mysql-release            0                                       
openzipkin/zipkin-mysql                                Mirror of https://quay.io/repository/openzip…   0                                       
astronomerio/mysql-sink                                MySQL sink                                      0                                       [OK]
ansibleplaybookbundle/mysql-apb                        An APB which deploys RHSCL MySQL                0                                       [OK]

```

## 2.然后从hub.docker.com或者第三方有加速的docker网站查看mysql镜像版本

**[阿里mysql docker镜像连接](https://dev.aliyun.com/detail.html?spm=5176.1972343.2.2.79825aaaVNcsUP&repoId=1239)**
```
8.0.3, 8.0, 8 (8.0/Dockerfile)
5.7.20, 5.7, 5, latest (5.7/Dockerfile)
5.6.38, 5.6 (5.6/Dockerfile)
5.5.58, 5.5 (5.5/Dockerfile)
```

## 3.我选择5.6.38版本下载

```
docker pull mysql:5.6.38
```

```
[root@iZ0p61por0uylmZ ~]# docker pull mysql:5.6.38
5.6.38: Pulling from library/mysql
f49cf87b52c1: Pull complete 
78032de49d65: Pull complete 
837546b20bc4: Pull complete 
9b8316af6cc6: Pull complete 
28dd7bab809d: Pull complete 
8b95be8b8d36: Pull complete 
67ee8c6f60b5: Pull complete 
74616d0d8b72: Pull complete 
6246d987d47e: Pull complete 
66cd90934fab: Pull complete 
Digest: sha256:078c9e0486639831d44c18ec8bc545e20ca74419822387c3eab2ea54a8e71515
Status: Downloaded newer image for mysql:5.6.38
```


## 4.查看本地镜像 可以看到已经在本地仓库

```
docker images
```

```
[root@iZ0p61por0uylmZ ~]# docker images
REPOSITORY                                     TAG                 IMAGE ID            CREATED             SIZE
mysql                                          5.6.38              15a5ee56ec55        6 weeks ago         299MB

```

## 5.开启mysql一个容器(就是我们平常所说的实例)

```
docker run --name mysql5.6.38 --restart=always -v /etc/localtime:/etc/localtime:ro -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql:5.6.38 --lower_case_table_names=1
```

```
-v /etc/localtime:/etc/localtime:ro 设置容器的时间与宿主机同步
--restart=always 随docker启动而启动
-e MYSQL_ROOT_PASSWORD=root 初始化密码
--lower_case_table_names=1 表名忽略大小写
```

```
[root@iZ0p61por0uylmZ ~]# docker run --name mysql5.6.38 -e MYSQL_ROOT_PASSWORD=root -d -p 3306:3306 mysql:5.6.38 --lower_case_table_names=1
1794aed7cfe17c7a8df7c1403a25feaecced3ccbef3844c9240bb0c8e27c2b85

[root@iZ0p61por0uylmZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
1794aed7cfe1        mysql:5.6.38        "docker-entrypoint.s…"   7 seconds ago       Up 6 seconds        0.0.0.0:3306->3306/tcp   mysql5.6.38
```

## 6.mysql远程连接

**navicat远程连接mysql容器 --> 外网地址:3306**
**如果是阿里云esc服务器,安全组3306端口要开放**


## 7.如果是在同一个docker下的容器要连接mysql
**7.1别名访问**
**比如tomcat连接mysql,那tomcat的启动参数里要加--link连接**
```
docker run --name tomcat重新命名 --link mysql5.6.38:别名 -d tomcat仓库名
```

```
在tomcat容器里的mysql连接地址就写为:别名
比如：jdbc:mysql://192.168.123.123:3306
就写为:jdbc:mysql://别名:3306
```
**7.2内网ip访问**
**获取mysql容器内网ip地址**

```
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mysql5.6.38
```

```
[root@iZ0p61por0uylmZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
432bd371973f        tomcat:8.5.24       "catalina.sh run"        12 minutes ago      Up 12 minutes       0.0.0.0:8080->8080/tcp   tomcat8.5.24
1794aed7cfe1        mysql:5.6.38        "docker-entrypoint.s…"   3 hours ago         Up About an hour    0.0.0.0:3306->3306/tcp   mysql5.6.38
[root@iZ0p61por0uylmZ ~]# docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mysql5.6.38
172.17.0.2
```

```
在tomcat容器里的mysql连接地址就写为:内网ip
比如：jdbc:mysql://192.168.123.123:3306
jdbc:mysql://172.17.0.2:3306
```

## 8.进入mysql容器

```
docker exec -it 容器id或者容器名字 /bin/bash
```

```
[root@iZ0p61por0uylmZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
1794aed7cfe1        mysql:5.6.38        "docker-entrypoint.s…"   29 minutes ago      Up 29 minutes       0.0.0.0:3306->3306/tcp   mysql5.6.38

[root@iZ0p61por0uylmZ ~]# docker exec -it 1794aed7cfe1 /bin/bash
root@1794aed7cfe1:/# 
```
## 拿到shell,收工
