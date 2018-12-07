---
title:  linux命令速查
date: 2018-12-06 19:30:27
categories:
- DevOps
---



# 查找

<!--more-->

``` 
find  /usr/bin -name *.zip
locate *.doc
whereis xx
whereis -b xx #二进制文件

```



# 文件

```
mkdir -p /xx/xx
touch xx.html
mv 
cp
cp -r test/ t/
rm -rf
ls -la

scp root@ip:xx.html  /xx
```



# 权限

```
chown -R user test/ #更改用户 
chmod -R 777
ls -s name linkname
```



# 输入输出重定向和管道

```
echo "xx" > xx.html #输出重定向到文件
echo "xx" >> xx.html #输出重定向到文件末尾

cat << 	EOF
xx
xx
EOF

cat << END >xx.html
xx
xx
END

#管道 | ，将一条命令输出连接到另一条命令输入
ls |grep xx

```



# 压缩

```
gzip xx.tar # 生成xx.tar.gz
gzip -d xx.tar.gz #解压

rar x xx.rar

tar -cvf xx.tar xx/  #c压缩 v进度 f文件名 z使用gzip
tar -xvf xx.tar #解压x 
```



# 用户

```
useradd -g group user1
passwd user1
groupadd gro

history

userdel -r user1
usermod -l user1 user2

id
su
sudo

```



# 进程

```
kill pid
kill -l
ps aux
top
lsof xx.html #文件的进程

nice
renice

/proc #目录下有系统内核相关信息
```



# 编辑器

```
vim 
:wq
:q!
/search #搜索

```

