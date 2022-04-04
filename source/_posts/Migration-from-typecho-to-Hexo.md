---
title: 从typecho迁移至Hexo
date: 2020-03-28 21:14:58
updated: 2021-03-03 20:00:00
tags: [Hexo,Typecho]
categories: 笔记
---
## 前言
很早就计划一次博客迁移，原来博客是使用typecho+handsome，在某些浏览器下访问会特别缓慢。
正好这几天有点时间，我就下定决心将博客从VPS迁移到腾讯COS并使用CDN让网站访问加速。
<!--more-->
经过一番搜索，最后决定了使用Hexo+NexT。
## 从typecho导出文章
由于typecho将Markdown的文章储存在数据库里，所以只要找到保存文章的表操作一下就可以轻松导出。
在这里我使用的是 [typecho2Hexo](https://github.com/NewbMiao/typecho2Hexo) 
它使用php编写，简单设置就能导出为Hexo专用的.md文件
代码如下
```php
<?php
// 运行 php converter.php
$db = new mysqli();
// 根据实际情况更改数据库地址,用户名,密码,数据库名
$db->connect('localhost','username','password','database');
$prefix = 'typecho_';
$sql = <<<TEXT
select title,text,created,category,tags from {$prefix}contents c,
 (select cid,group_concat(m.name) tags from {$prefix}metas m,{$prefix}relationships r where m.mid=r.mid and m.type='tag' group by cid ) t1,
(select cid,m.name category from {$prefix}metas m,{$prefix}relationships r where m.mid=r.mid and m.type='category') t2
where t1.cid=t2.cid and c.cid=t1.cid
TEXT;
$res = $db->query($sql);
if ($res) {
    if ($res->num_rows > 0) {
        while ($r = $res->fetch_object()) {
            $_c = @date('Y-m-d H:i:s', $r->created);
            $_t = str_replace('<!--markdown-->', '', $r->text);
            $_tmp = <<<TMP
title: {$r->title}
categories: {$r->category}
tags: [{$r->tags}]
date: {$_c}
---
{$_t}
TMP;
            // windows下把文件名从UTF-8编码转换为GBK编码，避免出现生成的文件名为乱码的情况
            if (strpos(PHP_OS, "WIN") !== false) {
                $name = iconv("UTF-8", "GBK//IGNORE", $r->title);
                echo $name.'<br>';
            } else {
                $name = $r->title;
				echo $name.'<br>';
            }
            // 替换不合法文件名字符
            file_put_contents(str_replace(array(" ", "?", "\\", "/", ":", "|", "*"), '-', $name) . ".md", $_tmp);
        }
    }
    $res->free();
}
$db->close();
```
将代码复制下来，简单修改一下数据库连接信息保存为converter.php。之后在保存的目录执行以下命令，你就可以看到目录下导出的.md文档。
```sh
php converter.php
```
得到这些.md文档后只需要放到Hexo的source/_post目录下即可。

## 在本地安装Hexo
### 安装Hexo-cli
>在开始安装hexo-cli之前要确保npm已安装且node.js版本符合要求

执行以下命令就可以全局安装hexo-cli
```sh
npm install hexo-cli -g
```
### 新建博客并下载主题
```sh
hexo init blog
cd blog
npm install
git clone https://github.com/theme-next/hexo-theme-next themes/next
```
然后在博客配置文件 _config.yml 将主题更改为 next 即可
```yaml
theme: next
```
执行```hexo s```可以在本地预览网站
## 部署至腾讯云COS
腾讯云COS是腾讯云的对象储存服务，其中就有一个静态网站功能可以将COS作为一个服务器来存放静态网站。
使用COS的好处是可以一条命令即可部署，且可以在COS上套上CDN加速网站整站访问。
**当然，要使用腾讯云的大陆CDN服务还需要一个已经备案的域名。**
### 创建储存桶
创建一个储存桶，访问权限选择**公有读私有写**
![-w698](https://cdn.gamesme.cn/2020/03/28/15854034045381.jpg)
然后在基础设置里启用**静态网站**
![-w492](https://cdn.gamesme.cn/2020/03/28/15854035485319.jpg)
在自定义域名中设置自己的域名
这样储存桶就创建好了
### 配置Hexo部署服务
将Hexo部署至腾讯云COS需要一个插件
在博客根目录下执行
```sh
npm install hexo-deployer-cos --save
```
然后编辑 _config.yml 下的 Deployment 设置
```yaml
deploy:
- type: qcloud-cos
  cosRegion: <Region>
  cosSecretId: <SecretId>
  cosSecretKey: <SecretKey>
  cosBucket: <Bucket-name>
  cosAppid:  <Appid>
```
如此设置完成，以后部署只需要运行以下命令就能轻松将网站一键部署到COS
```sh
hexo clean
hexo g -d
```

