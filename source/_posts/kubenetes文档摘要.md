---
title: kubenetes文档摘要
date: 2018-11-30 22:45:48
categories: 
- kubenetes
---

# 官网文档主页
<!--more-->
[官方文档](https://kubernetes.io/zh/docs/home/)

# Kubernetes Components
* kube-apiserver(master )
* etcd(master )
* kube-scheduler(master )
* kube-controller-manager(master )
* cloud-controller-manager(master)
* kubelet
* kube-proxy
* DNS
* Dashboard

# 配置文件(kubernetes objects)
定义yaml文件即定义了一个kubernetes objects
## namespaces
`kubectl get namespaces`
`kubectl --namespace=<insert-namespace-name-here> get pods`
```
# In a namespace
$ kubectl api-resources --namespaced=true

# Not in a namespace
$ kubectl api-resources --namespaced=false
```
## Labels and Selectors
`=  !=  in  notin`

```
selector:
  matchLabels:
    component: redis
  matchExpressions:
    - {key: tier, operator: In, values: [cache]}
    - {key: environment, operator: NotIn, values: [dev]}
```
## Annotations

TODO

# Taint 和 Toleration
目的：pod分配到合适的节点

# Kubernetes API 概述
```
API服务器可以设置 -enable-swagger-ui=true 来启用API界面，使用浏览器访问 /swagger-ui
不使用ui访问/swaggerapi
```


# DNS Pod 与 Service
在集群中定义的每个 Service（包括 DNS 服务器自身）都会被指派一个 DNS 名称。

## DNS 名字
`<service-name>.<namespace-name>.svc.cluster.local`



