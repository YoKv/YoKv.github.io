---
title:  rancher-k8s.md
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
## 重启

# 安装Rancher
## 版本
2.0.8，单节点安装，无证书

## master节点
带API审计，便于理解
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
点击添加集群



