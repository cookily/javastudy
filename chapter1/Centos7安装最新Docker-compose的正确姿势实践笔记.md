# Centos 7 安装最新 Docker-compose 的正确姿势 实践笔记
---


**参考：[docker-compose-官方文档](https://github.com/docker/compose/releases/)**
-------

**系统:Centos 7 64bit**
##还没装docker的，先移步[安装官方最新docker](https://blog.csdn.net/cookily_liangzai/article/details/80748138)




##  1.下载当前最新版本docker-compose
```
curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

## 2.赋予可执行权限

```
chmod +x /usr/local/bin/docker-compose
```

## 3.查看版本

```
docker-compose --version
```