---
title: Ubuntu与Windows之间的远程连接
date: 2017-06-13 15:34:47
categories: [操作系统,Linux]
---

之前使用Ubuntu 14.04时其实一直没有很好得解决远程桌面连接的问题，每次用到实验室的Windows服务器时，我都会切换到windows系统去使用。。可以说非常不方便了。这回重装Ubuntu16.04，总算是解决了这个问题，并且还解决了Windows连接Ubuntu的问题。

# Ubuntu连接Windows服务器
其实Ubuntu下也有类似Windows远程连接的很方便的自带软件，那就是remmina，remmina支持很多协议，包括rdp，vnc等等，我们这里选用rdp协议来连接windows服务器。

<!-- more -->
Remmina有着简单易懂的图形界面，建立连接很简单。事实上，之前用Ubuntu 14.04时我就用过remmina，但当时碰到了一个很棘手的问题，那就是只能建立远程连接，而无法传输文件。在设置中有一个共享文件夹的选项，但是即使勾选后在windows中依然无法显示（windows服务器版本为sever 2012R）。所幸经过一番搜索，我发现了问题的解决办法，只需要利用第三方软件源将remmina进行版本更新。
```
sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
sudo apt-get update
sudo apt-get install remmina remmina-plugin-rdp libfreerdp-plugins-standard
```
然后再重启remmina，就可以使用共享文件夹了。

# Windows连接Ubuntu
实验室的工作电脑我装的是Ubuntu，但是笔记本我装的是Windows系统，并且平时笔记本一般放在寝室。为了能在寝室用笔记本连接实验室的Ubuntu(就是爱学习！),我尝试了一些办法，我觉得最好的办法就是用teamviewer!

## Teamviewer安装
从[官网](https://www.teamviewer.com/en/download/linux/)下载deb文件（非商业用途的个人版本是免费的），然后执行命令（建议使用apt-get安装以解决依赖问题）
```
sudo apt-get install ./teamviewer*.deb
```

## Teamviewer使用

在Ubuntu中打开teamviewer后会生成了一个ID和密码，我们只要在windows段也打开teamviewer，输入该ID和密码就可以连接到Ubuntu了。注意，这种连接方式是需要联网的。

如果想要不联网，在内网中直接使用的话，我们需要设置`Extras->Options->General->Incoming LAN connections`选择accept exclusively,这样的话，就只会通过局域网连接了，ID也会显示为你的局域网中的ID。经过测试，我觉得teamviewer的连接速度也是挺给力的！赞！
