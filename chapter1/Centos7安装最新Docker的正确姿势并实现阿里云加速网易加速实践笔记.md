# Centos 7 安装最新 Docker 的正确姿势 并实现阿里云加速 网易加速 实践笔记
---


**参考：[阿里云官方文档](https://help.aliyun.com/document_detail/60742.html?spm=a2c4g.11186623.4.3.WBh3GJ)**
-------

**系统:Centos 7 64bit**
**直接用yum install docker -y安装的docker版本为1.12，但是docker发展很快，现在都17.12.0了。docker-ce是指docker的社区版**

# 挂在github上的个人博客：[由hexo强力驱动 个人博客](http://code.cookily.cn/2018/06/14/Centos7%E5%AE%89%E8%A3%85%E6%9C%80%E6%96%B0Docker%E5%B9%B6%E5%AE%9E%E7%8E%B0%E9%98%BF%E9%87%8C%E4%BA%91%E5%8A%A0%E9%80%9F%E7%BD%91%E6%98%93%E5%8A%A0%E9%80%9F/)


##  0.首先卸载旧版本docker及相关依赖
```
yum remove docker docker-common container-selinux docker-selinux docker-engine
```

## 1.安装必要的一些系统工具

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 2.添加软件源信息

```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## 3.更新并安装 Docker-CE

```
sudo yum makecache fast
sudo yum -y install docker-ce
```

## 4.开启Docker服务

```
sudo service docker start
```

## 5.验证是否安装成功

```
docker run hello-world
```

## 6.查看Docker版本

```
docker version
```
```
Client:
 Version:	17.12.0-ce
 API version:	1.35
 Go version:	go1.9.2
 Git commit:	c97c6d6
 Built:	Wed Dec 27 20:10:14 2017
 OS/Arch:	linux/amd64

Server:
 Engine:
  Version:	17.12.0-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.2
  Git commit:	c97c6d6
  Built:	Wed Dec 27 20:12:46 2017
  OS/Arch:	linux/amd64
  Experimental:	false
```

## 7.阿里云加速
**7.1首先注册开通阿里云开发者帐号**
**[跳转阿里官方 注册开通阿里云开发者](https://dev.aliyun.com/search.html)**

**7.2登录后在个人中心点击加速器，同时会给出加速器地址。**

**7.3选择对应的系统并根据自己的docker版本执行相应的步骤；**

**7.4Docker客户端版本大于1.10的用户 可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器：**

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://你的专有码.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 8.网易加速(个人感觉网易加速比较稳定)
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```