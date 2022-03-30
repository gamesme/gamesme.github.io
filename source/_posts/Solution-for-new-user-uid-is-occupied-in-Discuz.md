title: Discuz新用户注册UID被占用解决办法
categories: 分享
tags: [原创,discuz]
date: 2019-04-09 07:15:00
---
## 前言
群里有位大佬的Discuz论坛搬家了 
文件和数据库都原封不动的导入
但是注册新用户时 居然提示 UIDXXX被占用
<!--more-->

![1](https://i.loli.net/2019/04/09/5cac4b18cac34.png)
只好借助万能的百度

-------
## 解决方法 - 适用于UCenter数据库内不含Discuz的Members数据（或不一致）
请在数据库中执行以下sql查询语句
```sql
INSERT INTO `pre_ucenter_members` (
`uid` ,
`username` ,
`email` 
) 
SELECT `uid`,`username`,`email` FROM `pre_common_member` WHERE uid>2 AND uid<31312
```
对应的表名请根据实际情况修改
```INSERT```（插入）可改为```REPLACE```（替换）以解决对应UID已在数据表中存在的问题
```pre_ucenter_members```为UCenter用户数据表
```pre_common_member```为Discuz用户数据表
```WHERE uid>2 AND uid<31312```为用户UID范围
![2](https://i.loli.net/2019/04/09/5cac4c8c4e5c5.png)
-------
## 解决方法 - 适用于表前缀与配置文件不同
修改对应config的前缀为数据库中相应前缀


-------
## 解决方法 - 适用于表前缀与配置文件相同 且UC和Discuz中Members数据一致
经过一番探查 
发现config_*.php中的数据表前缀与数据库相同
且UCenter 与 Discuz 的用户数据表内容一致
于是乎打开两个数据表中的uid键值 -> 操作 选项卡
找到Auto_INCREMENT（自增） 修改为大于最大UID即可正常进行注册了
![3](https://i.loli.net/2019/04/09/5cac4b186feed.png)