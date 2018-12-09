---
title: kubenetes文档摘要（一）--概述
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
Annotations不会被Kubernetes直接使用，其主要目的是方便用户阅读查找。

## Field Selectors
支持`=  == !=`，主要以下几个标签:
* metadata.name=my-service
* metadata.namespace!=default
* status.phase=Pending

```
kubectl get pods --field-selector status.phase=Running
```
```
kubectl get pods --field-selector=status.phase!=Running,spec.restartPolicy=Always
```
```
kubectl get statefulsets,services --field-selector metadata.namespace!=default
```

# kubectl 命令
## 常用
```
kubectl create -f nginx.yaml
kubectl delete -f nginx.yaml -f redis.yaml
# 更新
kubectl replace -f nginx.yaml
# 更新配置
kubectl apply -R -f configs/

kubectl create service nodeport <myservicename>
```
## CRUD
### add
* run
* expose
* autoscale

### upadte
* scale
* annotate
* label
* set
* edit
* patch

### delete
* delete` <type>/<name>`

###  view
*  get
*  describe
*  logs

### example
```
kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run | kubectl set selector --local -f - 'environment=qa' -o yaml | kubectl create -f -
```
```
kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run > /tmp/srv.yaml
kubectl create --edit -f /tmp/srv.yaml
```

## 导出
```
kubectl get <kind>/<name> -o yaml --export > <kind>_<name>.yaml
```





