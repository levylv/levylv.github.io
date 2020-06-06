---
title: sudo与su
date: 2019-02-15 18:13:30
categories: [编程开发,Shell]
---



sudo -u user单纯以某用户身份运行命令，并不切换环境，需要输入当前用户密码，。sudo的各类权限在/etc/sudoers配置

 

su 则切换到某用户的环境下，需要输入某用户的密码。这里注意一定要加 su -  ，只有这样才会生成login shell加载/etc/profile和~/.bash_profile，如果不加的话，启动non-login shell，不加载上述文件，环境变量部分是原用户的。

