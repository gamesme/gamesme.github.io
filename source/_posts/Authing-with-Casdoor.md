---
title: 开源身份认证服务CasDoor
date: 2022-04-05 01:42:02
categories: 笔记
updated:
tags: [CAS]
---

## 前言

一直使用moodle作为内部的考试系统，原有的验证方式是账号密码，很不方便。moodle自带了OAuth2验证插件，由于钉钉提供的第三方登录接口返回的不是标准的OAuth2参数，一直在寻找如何接入钉钉登录的方法。

于是发现了这个go语言开发的Casdoor身份验证平台。

<!-- more -->

## 环境要求

- [Go 1.6+](https://go.dev/dl/)
- [Node.js LTS (16 or 14)](https://nodejs.org/)
- [Yarn 1.x](https://classic.yarnpkg.com/en/docs/install)
- Mysql

## 安装

克隆项目

```bash
cd path/to/folder
git clone https://github.com/casdoor/casdoor
```

修改[**conf/app.conf**](https://github.com/casdoor/casdoor/blob/master/conf/app.conf)中数据库参数

```ini
driverName = mysql
dataSourceName = root:123456@tcp(localhost:3306)/
dbName = casdoor
```

编译前端

```bash
cd web
yarn install
yarn build
```

编译后端

```bash
#项目根目录下
go build
./casdoor
```

项目默认监听8000端口，到此可以打开localhost:8000看看效果。

## 配置

Casdoor的用户(User)属于一个组织(Organization)，只能访问同一个组织(Organization)下的应用(Application)。

应用的(Application)认证方式可以是由casdoor用户(User)的账号密码，也可以接入第三方认证提供商(Provider)，使用如OAuth、SAML等协议进行认证。

### 组织

首先要在casdoor内创建组织，建议不要混用默认的build-in组织，这样对应应用下的用户就不会访问到casdoor的管理界面。

![组织界面](https://cdn.gamesme.cn/uPic/2022/04/05/XH8sdQWQMXd6.png)

修改显示名称，密码类型为md5+salt，填入salt值，这样用户密码就不会以明文储存。

### 应用

![应用](https://cdn.gamesme.cn/uPic/2022/04/05/EL28rccasdoor-application.png)

选择用户所属的组织，记录下来客户端ID(ClientID)与密钥

(ClientSecret)。如果仅接入第三方登录，可以关闭 开启密码 这个选项。

重定向Urls就是应用的回调地址，需要加入此处才能正常使用Casdoor认证身份。

启用保持登录会话，可以减少跳转到钉钉认证的次数。最后在提供商加入钉钉即可。

### 提供商

Casdoor支持的提供商有不少，在其官网有介绍。这里使用的是钉钉作为提供商。

![提供商](https://cdn.gamesme.cn/uPic/2022/04/05/ld0Rz1GjLnc0.png)

修改名称，而后填入在钉钉开发者中心获取的 AppID 与 AppSeceret 即可。

### 用户

创建用户时需要注意选择正确组织。

Casdoor提供了Excel批量导入用户的功能，但是不完善。经测试只实现了基本的xlsx->database，未针对导入的密码或者字段进行更新。如遇到已存在的条目就会出错无法导入。

### 题外话

Casdoor还提供了如 角色(Role) 权限(Permission) 资源(Resource) Webhook 同步器 等功能模块，还没来得及测试，待日后使用再来更新。
