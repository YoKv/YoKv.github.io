---
title: kubenetes文档摘要（三）
date: 2018-12-04 22:45:48
categories: 
- kubenetes
---



# Taint 和 Toleration

<!--more-->

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



