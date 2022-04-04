---
title: Mac迅雷手动精简插件
categories: 分享
tags: [macOS]
date: 2019-07-16 06:18:00
---
## 前言
在此目录下存放着Mac版本迅雷应用中心的插件
本来一个下载工具，就应该单纯的做好下载，天天整点花里胡哨的东西。


<!--more-->


``` text
/Applications/Thunder.app/Contents/PlugIns/ 
```

删掉对应的文件就可以精简相应的功能

```text
advertising （广告） 
featuredpage （主页） 
feedback （反馈） 
iOSThunder （手机迅雷） 
activitycenter （活动中心）
myvip （会员中心） 
softmanager （软件管家） 
viprenew （会员开通） 
viptips （会员提示） 
xlbrowser （内置浏览器） 
xlplayer （迅雷影音） 
livestream （直播）
```

针对不同的需求，可以酌情处理以下插件： 
需使用迅雷快鸟进行宽带提速的，请保留 ```bbassistant```
插件需要使用迅雷离线空间的，请保留 ```lixianspace``` 插件，不需要的可以删除；
需要使用会员权限的，请保留 ```viptask``` 插件，不需要的可以删除； 
需要登陆迅雷账户的，请保留 ```userlogin``` 插件，不需要的可以删除； 
需要使用内置的字幕下载功能的，请保留 ```subtitle``` 插件，不需要的可以删除； 
需要搭配浏览器使用的，请保留 ```browserhelper``` 插件，不需要的可以删除； 
下载宝（或玩客云）用户请保留 ```onethingcloud``` 插件，不需要的可以删除。

##效果图
![精简后的Mac迅雷](https://cdn.gamesme.cn/images/xl.png)
