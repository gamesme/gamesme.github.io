title: 纯净CentOS 7 安装BBR+Deluge
categories: 分享
tags: [原创,CentOS]
date: 2019-04-13 07:57:00
---
新撸了两台Hetzner的Dedicated Server拿来刷PT
并且想通过Appnode来管理
于是选择了安装CentOS 7
然后发现CentOS下居然没有Deluge
一番摸索于是有了这个教程


<!--more-->


-------
## 安装原版BBR
首先升级内核
```bash
#CentOS 7系统
#导入ELRepo公钥
wget https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm --import RPM-GPG-KEY-elrepo.org
#安装ELRepo
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
#升级最新内核
yum --enablerepo=elrepo-kernel install kernel-ml -y
#调整内核启动顺序
grub2-mkconfig -o /boot/grub2/grub.cfg && grub2-set-default 0
#重启
reboot
```
重启之后启用BBR
```bash
#查看最新内核，如果大于4.9，则进行下一步
uname -r
#修改配置
cat >>/etc/sysctl.conf << EOF
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
EOF
#使配置生效
sysctl -p
#检查生效，输出带有tcp_bbr 20480  0即生效
lsmod | grep bbr
```
-------
## 安装Deluge并启动WebUI
因为官方并无CentOS的Deluge包
于是使用第三方源安装（来自nux）
```bash
wget http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
rpm -ivh nux-dextop-release-0-5.el7.nux.noarch.rpm
yum -y install deluge-web
```
启动Deluge与WebUI
```bash
deluged
deluge-web --fork
```

-------
## 安装Flexget
安装pip与flexget
```bash
yum install -y python-pip
pip install --upgrade setuptools
pip install flexget
```
将配置文件config.yml编辑好```/root/.config/flexget/config.yml```
然后启动Flexget
```bash
flexget daemon start -d
```

到此为止 一台CentOS 7 系统的盒子就装好了
> BBR脚本来自 [萌鼠博客](https://www.moerats.com/archives/580/ "萌鼠博客")
Deluge 脚本来自 [微魔部落](https://www.vmvps.com/installing-deluge-and-deluge-web-on-centos-7.html "微魔部落")