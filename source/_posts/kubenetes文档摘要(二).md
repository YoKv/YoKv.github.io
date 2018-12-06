---
title: kubenetes文档摘要（二）
date: 2018-12-03 22:45:48
categories: 
- kubenetes
---

# Nodes

<!--more-->

Node Status:
* Addresses
* Condition
* Capacity
* Info

## Addresses
* HostName
* ExternalIP
* InternalIP

## Condition
`ready`的节点的状态：
* OutOfDisk
* Ready
* MemoryPressure
* PIDPressure
* DiskPressure
* NetworkUnavailable
 
## Capacity
资源的描述

## Info
节点描述，内核、版本信息、操作系统等。

# Master Node
管理权限的节点，主要通过apiserver获取其他节点信息：
	> 通过kubelet获取节点的pods和pods状态，提供kubelet的端口转发功能。
	> nodes, pods, and services通过http连接
	
# 镜像
## 私有镜像库
创建一个secret作为拉取镜像的凭证
```
apiVersion: v1
kind: Pod
metadata:
  name: foo
  namespace: awesomeapps
spec:
  containers:
    - name: foo
      image: janedoe/awesomeapp:v1
  imagePullSecrets:
    - name: myregistrykey
```

或者，在一台主机登录私有镜像库，将凭证`$HOME/.docker/config.json`复制到所有节点：

```
docker login [server]
nodes=$(kubectl get nodes -o jsonpath='{range .items[*].status.addresses[?(@.type=="ExternalIP")]}{.address} {end}')
for n in $nodes; do scp ~/.docker/config.json root@$n:/var/lib/kubelet/config.json; done
```

# 容器生命周期hook
* PostStart
* PreStop

# Pod
pod可以运行一个或多个容器，docker容器的网络模式是`None`，容器间可以使用`localhost`直接访问，磁盘也共同使用。

## pod的控制
* Deployment
* StatefulSet
* DaemonSet

## pod 状态
* Pending
* Running
* Succeeded
* Failed
* Unknown

## pod 探针（probes）
方式：
* ExecAction
* TCPSocketAction
* HTTPGetAction

类型：
* livenessProbe（是否存活）
* readinessProbe（是否暴露到svc）

```
        livenessProbe:
          httpGet:
            scheme: HTTPS
            path: /
            port: 8443
          initialDelaySeconds: 30
          timeoutSeconds: 30

```

## pod 生命周期
首先创建一个`pause`容器，分配IP。
启动其他定义的容器
如果有probes，根据probe获取容器状态。

删除pod：会有一个默认30s的缓冲期，让pod自行shotdown,程序正常退出，超出时间，则强制删除（kill），也可以直接强制删除。如果`preStop`的hook没有结束，延长2s。

## initContainers

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'until nslookup myservice; do echo waiting for myservice; sleep 2; done;']
  - name: init-mydb
    image: busybox
    command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']

```

## pod  preset
