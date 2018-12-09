---
title: kubenetes文档摘要（三）--网络
date: 2018-12-04 22:45:48
categories: 
- kubenetes
---

# services
<!--more-->
```
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376 #pod容器端口
```

## 不使用selectors的情况
* 指向其他namespace的serrvice
* k8s外部的服务
* 希望在生产中拥有外部数据库集群，但在测试中使用自己的数据库。？？？

```
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
    
    
---
kind: Endpoints
apiVersion: v1
metadata:
  name: my-service
subsets:
  - addresses:
      - ip: 1.2.3.4
    ports:
      - port: 9376
```

## DNS Pod 与 Service
在集群中定义的每个 Service（包括 DNS 服务器自身）都会被指派一个 DNS 名称。
同一个`namespace`中直接通过`service`名，不同`namespace`使用`serviceName.namespaceName`访问。

### DNS 名字
`<service-name>.<namespace-name>.svc.cluster.local`



## Headless services
`.spec.clusterIP:None`

## ExternalName
```
kind: Service
apiVersion: v1
metadata:
  name: my-service
  namespace: prod
spec:
  type: ExternalName
  externalName: my.database.example.com
```
访问`my.database.example.com`可以使用`my-service`代替，各个环境配置一致，不需要改代码( CNAME 重定向)
CNAME 重定向对于每个环境均使用相同端口的服务非常有效

##  External IPs
```
kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
  - name: http
    protocol: TCP
    port: 80
    targetPort: 9376
  externalIPs:
  - 80.11.12.10
```

