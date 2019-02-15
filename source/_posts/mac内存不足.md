---
title: mac内存不足
date: 2018-11-06 17:57:46
tags: 文档
category:
- 文档
---
#### 1. 清除docker垃圾文件
docker system prune命令可以用于清理磁盘，删除关闭的容器、无用的数据卷和网络，以及dangling镜像(即无tag的镜像)。docker system prune -a命令清理得更加彻底，可以将没有容器使用Docker镜像都删掉。注意，这两个命令会把你暂时关闭的容器，以及暂时没有用到的Docker镜像都删掉了…所以使用之前一定要想清楚吶。
```
docker system prune -a
```

#### 2. 清除APP缓存垃圾
常见目录`~/Library/Caches`, `du -sh *`查看所有文件的大小分布, `rm -rf `删除缓存文件。