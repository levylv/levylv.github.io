---
title: 系统管理工具Systemd
date: 2021-01-20
categories: [操作系统,Linux]
---



以前linux用的是initd工具来管理系统，后来渐渐被systemd取代了。具体可看[这篇博客](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)，介绍得非常详细。



注意以下几点：

- systemd是可以直接与系统内核交互的，比如关机，关电源，停止cpu计算等。
- unit是一个资源的概念，不仅仅包含service，还有很多其他的比如挂载，硬件设备等资源。
- target是一个unit组的概念，相当于以前的run_level。比如poweroff，reboot, multi_user，graphical等状态，这些状态都对应了一系列unit的启动和关闭，一般我们都用默认状态，对应的是graphical状态。
- 我们一般都是用户自定义damen，所以要加个后缀--user，这种情况下service的符号链接是注册在~/.config/systemd/user/下的。
- 用户自定义service的话，unit配置文件只要写【Service】和【Install】就好了。

