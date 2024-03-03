---
{"dg-publish":true,"permalink":"/CS计算机科学/Drupal/Drupal安装/","created":"2024-03-04T00:14:47.629+08:00","updated":"2024-03-04T01:06:51.072+08:00"}
---


[Local server setup | Develop | Drupal Wiki guide on Drupal.org](https://www.drupal.org/docs/develop/local-server-setup)

[wodby/docker4drupal: Docker-based Drupal stack (github.com)](https://github.com/wodby/docker4drupal)

[Local environment - Wodby Docs](https://wodby.com/docs/1.0/stacks/drupal/local/#usage)

## 使用Composer

其它安装方式几乎绕不过去，特别是有些复杂模块，还是得用 Composer

[Composer 中文文档 | Composer 中文网 (phpcomposer.com)](https://docs.phpcomposer.com/)

[Packagist/Composer中国全量镜像 (pkg.xyz)](https://pkg.xyz/)

[drupal-composer/drupal-project: :rocket: Composer template for Drupal projects](https://github.com/drupal-composer/drupal-project)

但是非常坑爹，Drupal的模块没有更新到官方镜像源，意味着阿里云只能通过代理访问。

composer走代理设置

`composer config -g  http_proxy [http://f128.kdltps.com:15818](http://f128.kdltps.com:15818)`

命令走不通

使用系统代理，快代理不能翻墙，免费代理寻找很麻烦。

目前变通办法：通过在海外的虚拟主机运行Terminal，通过Composer命令下载对应模块后，再转给其他服务器，还需要维护vendor目录等一致。本地电脑composer应该也行。

[使用composer安装Drupal及管理依赖（一） - 大伟哥博客 (daweibro.com)](https://www.daweibro.com/node/269)

[使用www-data用户运行Composer - 大伟哥博客 (daweibro.com)](https://www.daweibro.com/node/270)

使用www用户运行composer

`sudo su  - www -s /bin/bash`