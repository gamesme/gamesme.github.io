---
title: Plex刮削正确姿势
categories: 笔记
tags: [NAS,Plex]
date: 2021-07-01 15:54:00
---

## 前言
最近购置了一台 DS920+ ，配合以前购入的Plex永久订阅开始搭建家庭媒体服务器。

Plex自带的刮削器针对媒体文件命名有要求，而且刮削下来的meta信息参差不齐。

一番搜索发现了一套近乎完美的刮削体系。

<!--more-->

-------

## 准备

* [Plex 媒体服务器](https://www.plex.tv/) 

* [TinyMediaManager](https://www.tinymediamanager.org/)
* Plex插件[XBMCnfoMoviesImporter.bundle](https://github.com/gboudreau/XBMCnfoMoviesImporter.bundle) 用于读取电影NFO

* Plex插件[XBMCnfoTVImporter.bundle](https://github.com/gboudreau/XBMCnfoTVImporter.bundle) 用于读取电视节目NFO

## TinyMediaManager

从官网下载安装tmm(TinyMediaManager,下同)，一路下一步打开即可。

### 基础设置

首先打开设置添加媒体库的位置，电影与电视节目的操作类似。

<img src="https://cdn.gamesme.cn/images/tmm-setting-source.png" alt="tmm-setting-source"  />

选择刮削使用的数据来源，此处建议选择tmdb。imdb数据最全，但是对中文的支持性不是太好。

![tmm-setting-tmdb](https://cdn.gamesme.cn/images/tmm-setting-tmdb.png)

tmm内置了常见媒体服务器的预设设置，在这里我们选择Plex一键应用设置。

![tmm-setting-systemdefualt](https://cdn.gamesme.cn/images/tmm-setting-systemdefualt.png)

基本上保持默认即可使用，如果有特殊需求可以打开tmm官网文档查看高级设置。

### 基本使用

以电影文件为例，右键其打开搜索并刮削所选电影。

![tmm-scrap-movie-menu](https://cdn.gamesme.cn/images/tmm-scrap-movie-menu.png)

然后修正正确的电影名字，点击搜索即可刮削电影相关信息，并且自动写入nfo文件。

![tmm-search-movies](https://cdn.gamesme.cn/images/tmm-search-movies.gif)

刮削完成后，tmm还支持对文件进行批量重命名与整理。选中电影单击重命名&清理即可。很方便

![tmm-rename-movies](https://cdn.gamesme.cn/images/tmm-rename-movies.gif)

针对电视节目也是类似的操作。



## Plex 插件

通过tmm对视频文件刮削整理后，Plex的在线刮削器就可抛弃了。

下载好两个插件后，将其解压出来的.bundle文件夹放置到PlexMediaServer的指定目录下然后重启PlexMediaServer即可。

![image-20210701163459804](https://cdn.gamesme.cn/images/plex-plugins-dir-syno.png)

图中群晖系统Plex插件目录

```Plex/Library/Application Support/Plex Media Server/Plugins ```

### XBMCnfoMoviesImporter



接下来就能在 Plex设置-代理-电影 启用这个插件。

![image-20210701163705466](https://cdn.gamesme.cn/images/plex-plugin-xmbcinfo-movie.png)

在设置中勾掉这个选项

![image-20210701164009941](https://cdn.gamesme.cn/images/plex-xmbcnfo-setting.png)

最后在资料库设置选择这个代理即可

![image-20210701164140533](https://cdn.gamesme.cn/images/plex-media-library-movie-xmbcnfo.png)

### XBMCnfoTVImporter

在 Plex设置-代理-电视节目 启用这个插件

![image-20210701164412500](https://cdn.gamesme.cn/images/plex-plugins-xmbcnfo-tv.png)

![image-20210701164642921](https://cdn.gamesme.cn/images/plex-tv-xmbcnfo-enable.png)

这个插件默认会根据tag生成collection，目前还没发现在哪儿可以禁用这个设置。

### 默认封面是电影缩略图解决

只需要打开电影菜单，选择修正匹配/取消匹配后再次匹配即可正确加载nfo信息。

![pms-movie-matching](https://cdn.gamesme.cn/images/pms-movie-matching.gif)

