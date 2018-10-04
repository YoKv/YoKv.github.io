---
title:  rancher-k8s部署微服务demo
date: 2018-10-03 21:00:46
categories: 
- kubernetes
---

# 环境准备
<!--more-->
## 虚拟机
系统为centos7,4c4g
三个虚拟机：
* master 192.168.66.131
* worker1 192.168.66.138
* worker2 192.168.66.139

## 安装docker
略

## 关闭防火墙和selinux
```
sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
systemctl stop firewalld.service && systemctl disable firewalld.service
```

## 修改hostname
因为K8S的规定，主机名只支持包含 - 和 .(中横线和点)两种特殊符号，并且主机名不能出现重复
```
vi /etc/hostname
分别修改为master worker1 worker2
```
并且在hosts添加
```
192.168.66.131 master
192.168.66.138 worker1
192.168.66.139 worker2
```

## 时间和语言
```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

sudo echo 'LANG="en_US.UTF-8"' >> /etc/profile;source /etc/profile
```
完成后重启，环境准备就绪

# 安装Rancher
版本2.0.8，单节点安装，无证书

安装Server，带API审计
```
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /root/var/log/auditlog:/var/log/auditlog \
  -e AUDIT_LEVEL=3 \
  -e AUDIT_LOG_PATH=/opt/r-data \
  -e AUDIT_LOG_MAXAGE=20 \
  -e AUDIT_LOG_MAXBACKUP=20 \
  -e AUDIT_LOG_MAXSIZE=1000 \
  rancher/rancher:latest
```
浏览器访问`192.168.66.131` 
![](/images/rancher-init.png)

点击添加集群，选择`CUSTOM`
在master节点勾选etcd，Control，Worker
在worker节点勾选Worker
分别执行相关命令即可完成安装
![](/images/init-rancher-cluster.png)
![](/images/init-rancher-done.png)

# 部署spring cloud 微服务

## 点击部署服务
![](/images/rancher-deploy.png)

## 部署eureka
![](/images/rancher-eureka.png)

## 部署app
![](/images/rancher-app-a.png)
![](/images/rancher-app-b.png)
![](/images/rancher-app.png)
![](/images/rancher-eureka-ui.png)
部署成功，查看日志确认服务调用成功