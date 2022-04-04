---
title: macOS下Laravel开发环境的部署
categories: 笔记
tags: [macOS,Laravel]
date: 2019-09-05 02:52:00
updated: 2022-04-04 22:46:00
---
## 前言
突发奇想重装了Macbook，记录一下重新部署Laravel开发调试环境的过程。

> 本文已更新部分命令，加入了xdebug的配置

<!--more-->

**本文所使用的软件包如下**

* homebrew 3.4+
* Laravel 9.0+
* Valet 3.0+
* composer 2.3+
* xdebug 3.0+
* PhpStorm 2021.3


## 安装包管理器Homebrew
Homebrew是一个MacOS下的包管理器，类似于CentOS上的yum。
安装十分简单，只需一条指令即可完成。
将下列命令粘贴至终端内运行
```Bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
完成后即可使用```brew install php```等命令进行软件包的安装。
## 安装Laravel与Valet
Valet 是 Mac 极简主义者的 Laravel 开发环境。没有 Vagrant，不需要配置 /etc/hosts 文件。 甚至可以使用本地隧道公开分享你的站点。
安装Valet前需要确保没有其他程序占用80/443端口。

> 如使用Docker Desktop 推荐 Laravel Sail 安装

### 安装步骤
* 使用Homebrew安装php ```brew install php```
* 使用Homebrew安装Composer ```brew install composer```
* (可选)Composer 配置[phpcomposer](https://pkg.xyz/)镜像源 ```composer config -g repo.packagist composer https://packagist.phpcomposer.com```
* 安装Laravel 
  * （使用Composer）```composer global require laravel/installer``` 
  * （使用Sail） ```curl -s "https://laravel.build/example-app?with=mysql,redis" | bash```

* 使用Composer安装Valet ```composer global require laravel/valet``` 
* 将Composer加入系统的“PATH”变量中
* 运行 ```valet install``` 来配置和安装Valet与DnsMasq，并注册服务随系统自动启动。

安装完成后可以尝试```ping foobar.test```，只要返回```127.0.0.1```的响应则证明配置正确。[Valet文档](https://laravel.com/docs/9.x/valet)

## 安装数据库Mysql和Redis
安装数据库也十分简单，使用Homebrew一条命令即可安装数据库
```Bash
brew install mysql redis
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

## 安装xdebug
使用pecl来安装xdebug
```bash
## macOS x86
pecl install xdebug
## Apple Silicon
arch -x86_64 sudo pecl install xdebug
```
pecl会自动下载源码编译，并加载到```php.ini```

需要注意当前使用的php版本

### 配置xdebug与PhpStorm

打开对应版本的```php.ini```加入以下内容(适用于xdebug3+)

```text
[xdebug]
zend_extension="xdebug.so"
xdebug.mode=debug
xdebug.client_host=127.0.0.1
xdebug.client_port="9003"
```

打开PhpStorm>偏好设置>PHP>xdebug，确认调试端口包含```9003``` (按需)

***若使用 Valet ，需要将 ```~/.composer/vendor/laravel/valet/server.php``` 加入项目文件内***

否则 xdebug 无法正常打断程序运行

下载 xdebug-helper [Chrome](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc) [FireFox](https://addons.mozilla.org/firefox/addon/xdebug-helper-for-firefox/)

在插件选项内配置 ```IDE Key``` 为 ```PHPSTORM``` 









