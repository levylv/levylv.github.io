---
title: shell交互式及登录式
date: 2019-02-15 18:13:30
categories: [编程开发,Shell]
---

登录式和非登录式区别：

 

“登陆shell”通常指的是：（/etc/profile,~/.bash_profile, ~/.bashrc,/etc/bashrc）

-  用户通过输入用户名/密码（或证书认证）后启动的shell；

- 通过带有-l|--login参数的bash命令启动的shell。

例如，系统启动、远程登录、使用su -切换用户、通过bash --login命令或 -i 启动bash等。

 

而其他情况启动的shell基本上就都是“非登陆shell”了。(~/.bashrc, /etc/bashrc)

例如，从图形界面启动终端、使用su切换用户、通过bash命令启动bash等。 

 

“登录shell”和“非登陆shell”的区别在于**启动shell时所执行的startup文件不同**。

 

交互式和非交互式区别：

非交互式：有-c选项或者执行一个shell脚本

交互式： 命令行交互形式



注意：非交互式shell应用基本都是原shell环境下创建一个非交互式子shell执行程序，用户不变，环境变量不变，不会重新加载startup文件。

 

总结：

日常用到的就这三类：

交互式登录shell

交互式非登录shell

非交互式shell

 