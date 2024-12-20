---
{"dg-publish":true,"permalink":"/CS计算机科学/运维部署/Network/VSCode Tunnel：一行命令部署远程开发服务器/","noteIcon":"","created":"2024-08-28T17:26:35.000+08:00","updated":"2024-04-28T14:19:02.000+08:00"}
---

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.vpslog.org](https://blog.vpslog.org/blog/vscodetunnel/)

> 本篇教程基于 VPSLOG 公益服务器 SERVER11-VPS1，NAT / 纯 V6 / 无公网 服务器可用。

VSCode Tunnel：一行命令部署远程开发服务器

本篇教程基于 VPSLOG 公益服务器 SERVER11-VPS1，**NAT / 纯 V6 / 无公网 服务器可用。**

Introduction
------------

这一篇和上一篇文章 [使用 Codespace 一键开启云端编程体验](https://vpslog.net/blog/%E4%BD%BF%E7%94%A8-codespace-%E4%B8%80%E9%94%AE%E5%BC%80%E5%90%AF%E4%BA%91%E7%AB%AF%E7%BC%96%E7%A8%8B%E4%BD%93%E9%AA%8C/) 比较类似。上一篇文章主要讲述如何在没有云服务器的情况下获取远程开发环境，这篇文章关注在 VPS/NAS 上进行部署。

VSCode Remote 这个功能很早之前就有了，当时还是 SSH 连接，宿主机需要有公网 IP，且需要用 ssh key，否则每次打开都要输入密码。比较麻烦。之后 VSCode Tunnel 功能出现，真正实现了一键部署远程服务器。

使用 VSCode Tunnel 开启的 VSCode Server 远程开发服务器，**全部绑定在 Github 账户下，之后在任意联网设备上打开 vscode.dev，登录账号，都可以随时进行连接,** 而不需要下载 VSCode 桌面版。

由于所有流量都是走 Github Tunnel（实际上就是走微软 Azure 云代理），所以对服务器的网络环境基本没有要求，**不管是 NAT 还是 V6 服务器，一行命令就能用**。相较之前 SSH 确实方便很多。

How to use VSCode Tunnel
------------------------

登陆服务器，运行以下命令

```
curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz
tar -xf vscode_cli.tar.gz
./code tunnel
```

不出意外将有以下输出，注意其中被我马赛克的部分，打开 [Github 认证](https://github.com/login/device)，把你的认证代码复制进去。

![](/img/user/Z-attach/image.png)

![](/img/user/Z-attach/image-1.png)

然后一路确认即可。之后回到服务器，会要你给设备起个名字，随意。起好之后，打开 [https://vscode.dev/tunnel/](https://vscode.dev/tunnel/)[名字] 就可以直接体验 Web 版的 VSCode 了。

![](/img/user/Z-attach/image-2.png)

![](/img/user/Z-attach/image-3.png)

关闭命令窗口，VSCode Server 服务器也会关闭，如果需要再次打开，请使用

Conclusion
----------

吃灰小鸡的一种利用方式？命令合在一起就是`curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz && tar -xf vscode_cli.tar.gz && ./code tunnel`

* * *

最后修改于 2023-06-14