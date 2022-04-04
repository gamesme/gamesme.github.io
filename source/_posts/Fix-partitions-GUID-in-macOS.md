---
title: Mac修复引导失败(GUID磁盘)
categories: 笔记
tags: [macOS]
date: 2019-04-08 04:26:00
---
## 前言
之前装了一个 Deepin 系统，发现用不习惯。

于是在磁盘管理直接把 Ext4分区格式化了想合并两个分区把SSD 变为一整块。

结果悲剧就这么开始了...


<!--more-->


在网上下载了试用版的 Hard Disk Manager，一番操作无果，结果发现这个软件并不识别 APFS 格式的分区...

![修复](https://cdn.gamesme.cn/images/Mac-xiufu-boot-soft.png)


(我没注意看...仅支持10.12的 Sierra 然而我用的是10.14的 Mojave)



之后在网上找到了能合并APFS 容器命令的<a href="https://en.ihowto.tips/osx-apps-download-tutorials-tips-hacks-news/merge-apfs-containers-in-single-partition-macos-high-sierra-or-mojave.html" target="_blank" rel="noopener">教程</a>



## 第一步 删除空的分区且不格式化


```bash
sudo diskutil eraseVolume "Free Space" %noformat% /dev/disk0s3
```

![](https://cdn.gamesme.cn/images/Mac-xiufu-boot-2.jpg)

## 第二步 调整APFS 容器的大小

```bash
diskutil apfs resizeContainer disk0s2 0
```

![](https://cdn.gamesme.cn/images/Mac-xiufu-boot-3.jpg)


但是当我进行第二步的时候，提示无法识别磁盘格式(Unknow Partition Format)



一开始还不知道是什么问题，就用人生三大法则之一的

> 重启试试

我把电脑重启了...然后..就出现了带问号的文件夹...

MacOS 的引导丢失了...

还好手头有 PE..尝试修复引导无果..

然后使用 DiskGenius 发现系统分区的 分区类型GUID 变成了

FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF

![](https://cdn.gamesme.cn/images/Mac-xiufu-boot-3.jpg)

(图中红框的那个东西)

一番百度，最后找到了<a href="https://bbs.feng.com/read-htm-tid-7296511.html" target="_blank" rel="noopener">这个教程</a>

```bash
list disk 
select disk 0 
list partition 
select partition 2 
set id=7C3457EF-0000-11AA-AA11-00306543ECAC
```

使用 PE 启动后，打开命令提示符输入以上的命令就解决了(磁盘位置根据实际情况填写)



![](https://cdn.gamesme.cn/images/MAc-xiufu-boot-4.jpg)


最后重启，熟悉的Apple Logo 出现了。

