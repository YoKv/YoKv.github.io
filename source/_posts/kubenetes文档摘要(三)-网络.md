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

# DNS for Services and Pods
略

# Connecting Applications with Services
## pod的网络模型
docker的网络需要在本机创建NAT，并且把端口分配在主机的ip上，会造成端口冲突
k8s的给pod分配ip，pod内的容器使用docker的`None`网络，所以pod内是共享网络的，可以把pod抽象为一组紧耦合的服务集合。

Services的`ClusterIP`可以在集群被使用。pod的ip也可以在集群内使用。（pod怎么解决端口冲突？）

## service proxies


# Ingress
Ingress能让services通过http链接访问到（定义了路由）。
Ingress不暴露端口或者其他协议。
* Kubernetes as a project currently supports and maintains GCE and nginx controllers.
* Contour is an Envoy based ingress controller provided and supported by Heptio.
* F5 Networks provides support and maintenance for the F5 BIG-IP Controller for Kubernetes.
* HAProxy based ingress controller jcmoraisjr/haproxy-ingress which is mentioned on the blog post HAProxy Ingress Controller for Kubernetes. HAProxy Technologies offers support and maintenance for HAProxy Enterprise and the ingress controller jcmoraisjr/haproxy-ingress.
* Istio based ingress controller Control Ingress Traffic.
* Kong offers community or commercial support and maintenance for the Kong Ingress Controllerfor Kubernetes.
* NGINX, Inc. offers support and maintenance for the NGINX Ingress Controller for Kubernetes.
* Traefik is a fully featured ingress controller (Let’s Encrypt, secrets, http2, websocket), and it also comes with commercial support by Containous.

## 例子
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /testpath
        backend:
          serviceName: test
          servicePort: 80
```
## 最简单例子
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
spec:
  backend:
    serviceName: testsvc
    servicePort: 80
```

## 路由重写
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: simple-fanout-example
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        backend:
          serviceName: service1
          servicePort: 4200
      - path: /bar
        backend:
          serviceName: service2
          servicePort: 8080
```

	> foo.bar.com -> 178.91.123.132 -> / foo    service1:4200
	                                   / bar    service2:8080

## 基于host
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: name-virtual-host-ingress
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - backend:
          serviceName: service1
          servicePort: 80
  - host: bar.foo.com
    http:
      paths:
      - backend:
          serviceName: service2
          servicePort: 80
```
	> foo.bar.com -> 178.91.123.132 -> / foo    service1:4200
	                                   / bar    service2:8080



# Network Policies
网络策略
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

# hosts
类似于docker-compose的`extra_hosts`
```
apiVersion: v1
kind: Pod
metadata:
  name: hostaliases-pod
spec:
  restartPolicy: Never
  hostAliases:
  - ip: "127.0.0.1"
    hostnames:
    - "foo.local"
    - "bar.local"
  - ip: "10.1.2.3"
    hostnames:
    - "foo.remote"
    - "bar.remote"
  containers:
  - name: cat-hosts
    image: busybox
    command:
    - cat
    args:
    - "/etc/hosts
```