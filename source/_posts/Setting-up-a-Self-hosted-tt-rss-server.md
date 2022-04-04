---
title: 搭建自己的RSS服务
categories: 笔记
tags: [RSS]
date: 2019-06-11 10:11:00
---
## 前言
Tiny Tiny RSS（也可简称TT-RSS）

项目主页： https://tt-rss.org/


<!--more-->


优点/特性：

 1. 可自定义同步间隔；
 2. 订阅源管理便捷、支持导入OPML；
 3. 可自定过滤规则；
 4. 安装主题/插件非常简单；
 5. 最重要的：目前仍处于比较活跃的开发中

## 开始部署
环境要求
PHP 5.4 或以上；
PostgreSQL（9.1 或以上）或者 MySQL（必须支持InnoDB, TT-RSS不支持MyISAM）；

官方部署文档
https://git.tt-rss.org/git/tt-rss/wiki/InstallationNotes

下载程序并开始安装
在想要安装TT-RSS的网站根目录，执行git clone:

```bash
git clone https://tt-rss.org/git/tt-rss.git .
```
访问TT-RSS安装程序：

http://yoursite/tt-rss/install/
你会看到如下的安装界面：

填写完成后就会自动生成config.php的代码，你可以选择由安装程序自动生成config.php文件，或者自己复制后手动生成config.php文件。

对于该文件，通常我们不需要进行额外的参数设置，不过如果你需要加入邮件提醒、用户注册等功能，可以参考其中的注释，自己进行参数设置。

至此，TT-RSS其实已经安装完成了！

## 配置后端
> 官方文档：https://git.tt-rss.org/git/tt-rss/wiki/UpdatingFeeds

在这里，我采用了第二种方法，即crontab的方式。
这里需要注意的是更新脚本文件update.php或update_daemon2.php不能由root用户来执行，可以通过apache或者Nginx的运行用户来执行，如apache或者www等。
比如我们为www用户加入crontab

```sh
crontab -u www -e
```
添加如下内容：

```text
*/30 * * * * /usr/local/php/bin/php /data/wwwroot/安装TT-RSS的目录/update.php --feeds --quiet
```
需要注意：
上方的/usr/local/php/bin/php也可能为/usr/bin/php，具体以你实际为准；
同时也要注意上文提到的config.php中的路径需与上面统一；
> */30 代表每30分钟执行一次更新，你也可以按自己需求设定其他间隔。

### 设置

以上完成后，我们像正常访问网站一样打开TT-RSS，默认账户密码:

```text
username: admin, password: password
```
首次登录后请及时修改密码。同时建议创建新的使用者账户作为日常使用，与管理员账户隔开。

## 安装主题
Tiny Tiny RSS安装主题非常简单，只需将CSS文件放入themes目录下，并在后台进行选择即可。
目前看到仿Feedly和仿Google Reader的这两款主题比较受到大家欢迎:

tt-rss-feedly-theme
https://github.com/levito/tt-rss-feedly-theme
clean-greader
https://github.com/naeramarth7/clean-greader
以安装tt-rss-feedly-theme为例，先下载主题：

```bash
wget https://github.com/levito/tt-rss-feedly-theme/archive/master.zip
```
解压缩：

```sh
unzip master.zip
#复制主题至themes子目录
cp tt-rss-feedly-theme-master/feedly.css /themes
cp tt-rss-feedly-theme-master/feedly /themes
```
而后我们至后台启用即可。

### 拓展

以下拓展的解释来自：http://ju.outofmemory.cn/entry/39790
>af_unburn:解决feedburner等rss链接跳转;
bookmarklets:在设置-信息源生成bookmarklets标签;
embed_original:图标插件，点击图标会显示文章原始内容，而不是rss;
fever:模拟fever api，在设置-Fver
Emulation，设置好密码，可以和tt-rss的登录密码不同，然后就能支持fever的客户端比如reeder、Mr. Reader;
ff_feedcleaner:feed广告过滤，在设置标签生成FeecCleaner标签，过滤规则要用正则表达式，比较复杂;
googlereaderkeys:模拟google reader快捷键，如J、K等;
import_export:在设置-信息源，导入导出配置;
mail:图标插件，点击通过邮件分享;
mark_button:文章右下角能够快速将文章标记为已读未读;
mobilize:图标插件，点击显示readability简化的页面；
note:图标插件；
nsfw:根据标签隐藏文章内容；
share:图标插件，点击生成唯一的url方便分享；
swap_jk:添加j、k快捷键，类似vim



