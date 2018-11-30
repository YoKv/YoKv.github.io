---
title: kubeadm安装
date: 2018-11-28 22:45:48
categories: 
- kubenetes
---

# 环境
* centos7

# 安装指定版本docker-ce
```
yum -y install net-tools wget
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo  https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce-18.06.1.ce

mkdir /etc/docker
cat <<EOF >/etc/docker/daemon.json
{
  "registry-mirrors": ["https://7yyhdik3.mirror.aliyuncs.com"]
}
EOF

chkconfig docker on
systemctl daemon-reload
systemctl restart docker

```


# 系统环境配置
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl net.bridge.bridge-nf-call-iptables=1
sysctl net.bridge.bridge-nf-call-ip6tables=1
sysctl --system
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
swapoff -a 
systemctl stop firewalld
systemctl disable firewalld
```

# 安装
```
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
systemctl enable kubelet.service && systemctl start kubelet
```

# 修改hosts

# master初始化集群
```
kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

```

# worker加入集群
```
kubeadm join 192.168.154.132:6443 --token 77x4t4.q9kbeuokk1fx1j3q --discovery-token-ca-cert-hash sha256:cd059438d9f1f4b14286ef6a23b6dd497c8a30435a99b0b6471feb3afae8737c

kubectl label node kube-2 node-role.kubernetes.io/worker=worker
```

# dashboard
使用kube-proxy访问
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml

kubectl proxy --address='0.0.0.0' --disable-filter=true

http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
```


# 注意点

宿主机reboot后，集群没有自动重启，执行命令：
```
 swapoff -a
```
永久关闭swap:
```
vi /etc/fstab
注释行：
# /dev/mapper/centos-swap swap                    swap    defaults        0 0
```



# 资源链接

[官方文档](https://kubernetes.io/docs/setup/independent/install-kubeadm/)