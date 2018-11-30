---
title: minikube安装
date: 2018-11-28 21:45:48
categories: 
- kubenetes
---

# minikube 介绍
minikube能够快速搭建本地k8s，依赖于`docker-machine`，默认使用VirtualBox创建节点，在win10下支持`hyperv`

# 环境
* centos7
* docker-ce
* docker-machine

# 安装
```
yum install -y wget
wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo -O      /etc/yum.repos.d/virtualbox.repo
yum install -y gcc kernel-devel kernel-headers VirtualBox-5.2
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && sudo install minikube-linux-amd64 /usr/local/bin/minikube
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/linux/amd64/kubectl \
  && chmod +x kubectl && sudo cp kubectl /usr/local/bin/ && rm kubectl
minikube version
minikube start
minikube dashboard
```