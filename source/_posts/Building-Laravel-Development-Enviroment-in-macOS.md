title: macOS下Laravel开发环境的部署
categories: 笔记
tags: [macOS,原创,Laravel]
date: 2019-09-05 02:52:00
---
## 前言
全新安装的macOS想要部署一个Laravel的开发调试环境


<!--more-->


## 安装包管理器Homebrew
Homebrew是一个MacOS下的包管理器，类似于CentOS上的yum。
安装十分简单，只需一条指令即可完成。
将下列命令粘贴至终端内运行
```Bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
完成后即可使用```brew install php```等命令进行软件包的安装。
## 安装Valet与PHP
Valet 是 Mac 极简主义者的 Laravel 开发环境。没有 Vagrant，不需要配置 /etc/hosts 文件。 甚至可以使用本地隧道公开分享你的站点。
安装Valet前需要确保没有其他程序占用80/443端口。

### 安装步骤
* 使用Homebrew安装php ```brew install php```
* 使用Homebrew安装Composer ```brew install composer```
* 使用Composer安装Valet ```composer global require laravel/valet```
* 确保Composer在系统的“PATH”变量中
* 运行 ```valet install``` 来配置和安装Valet与DnsMasq，并注册服务随系统自动启动。

安装完成后可以尝试```ping foobar.test```，只要返回```127.0.0.1```的响应则证明配置正确。有关Valet命令的使用建议百度，此处不再赘述。

## 安装数据库Mysql和Redis
安装数据库也十分简单，使用Homebrew一条命令即可安装数据库
```Bash
brew install mysql@5.7 redis
```
安装完成后别忘了使用```brew services start mysqld/redis```启动服务。 

## 安装Mailhog
MailHog是一个本地邮件测试服务，它提供了一个Web界面可以检查应用发送的邮件。
使用Homebrew安装MailHog
```Bash
brew install mailhog
```
### MailHog配置
MailHog本身无需任何配置，在网站/应用的SMTP配置填写如下信息即可收到所有邮件
```Text
MAIL_DRIVER=smtp
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ADDRESS=no-reply@example.com
MAIL_NAME=No-reply
MAIL_ENCRYPTION=
```
之后打开```http://127.0.0.1:8025/```即可查看收到的邮件