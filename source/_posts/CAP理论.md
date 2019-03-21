---
title: CAP理论
date: 2019-03-21 19:30:27
categories:
- Java进阶
---

## 理论

<!--more-->

CAP理论：一个分布式系统最多只能同时满足一致性（Consistency）、可用性（Availability）和分区容错性（Partition tolerance）这三项中的两项

分区容错性：数据项复制到多个节点上



##  AP与CP

因为分布式系统是多个模块的，所以必须满足分区容错性（Partition tolerance），所以一般只有AP和CP两种。（CAP角度思考elasticsearch架构）