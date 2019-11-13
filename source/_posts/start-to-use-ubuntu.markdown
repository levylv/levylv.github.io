---
title: "开始使用Ubuntu"
date: 2016-04-19 20:41:51
tags: Ubuntu 
---
对linux的向往由来已久，加上前阵子在win7下用vim总是感觉很不舒服，用gcc编译还要专门去下载MinGW(minmalist GNU for Windows),这么想还不如直接去用linux，GNU下的那些工具就直接能用了！在linux下打造一个IDE吧！
> GNU's Not Unix!

哈哈哈，其实以前也装过一阵Ubuntu,但是那会啥都不会，四处碰壁，没用多久就泄气了。可是看了各种大牛的书后，发现windows这种操作系统都是给大牛们摸透了计算机后想办法降低门槛给小白用的，所以要是不会Linux，永远进不了真正的程序员世界。

<!-- more -->

对于linux的众多发行版中，我选择了Ubuntu 14.04 LTS，毕竟Ubuntu有不错的GUI环境（X window的gnome），我既不是忠实的GUI党也不是忠实的CLI党，我觉得选择自己最好用的才是最重要的，该GUI的时候还是得GUI（用命令行去找分区里的文件太痛苦了。。），哈哈当然CLI是超强大的！

大概花了两周左右的时间，将[《鸟叔的Linux私房菜之基础学习篇》](http://linux.vbird.org/)看了一半（实在好长。。剩下的慢慢看），然后将Ubuntu下的工作环境都部署了一遍，感觉以后大部分的工作都可以完成了。

这部署的过程相当纠结，当然主要还是因为我的强迫症，比如一个字体好不好看我要纠结半天，各种换啊换，每次强迫症犯的时候都好想打死自己啊！所以部署的效率实在是有点低诶。好了，话不多说，下面贴上我的配置。


# 分区

装ubuntu的时候要给系统分区，我参考了鸟叔的书以及网络上的一些博客后，最后我分区如下:
![Ubuntu 分区](/images/my/1.png)

因为我装了双系统，我把sda1,sda2,sda3三个主分区都分给了windows，sda4扩展分区分成了好多逻辑分区，分别给/boot，/，/home，/var，/usr，总共大概给Ubuntu650G左右^0^


# VPN

由于用的是校园网，学校有专门的VPN客户端，我就去学校的论坛那里下了个Ubuntu下的vpn客户端，配置简单，没费什么事就能上网了（当然，前提要设置好IP地址和DNS，连上校内网）。

# 换源

虽然进入系统后，软件更新源里默认的是中国的服务器，但我还是按照[Ubuntu中文](http://wiki.ubuntu.org.cn/%E6%BA%90%E5%88%97%E8%A1%A8)的推荐换了网易，搜狐，阿里云及我们自己学校的源。

```
#网易
deb http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.163.com/ubuntu/ trusty-backports main restricted universe multiverse

#搜狐
deb http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.sohu.com/ubuntu/ trusty-backports main restricted universe multiverse

#学校
deb http://mirrors.zju.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ trusty-backports restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse

#阿里云
deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse
```

# 输入法

Ubuntu下自带的ibus输入法用起来还是不舒服，不像搜狗输入法那样将模糊语义，关键词之类的做得符合日常生活习惯，所以我还是装了搜狗输入法。

可以直接去[官网](http://pinyin.sogou.com/linux/)下载deb包，然后`sudo dpkg -i *.deb` 像搜狗输入法是需要fcitx做输入法框架的，所以安装完后需要在语言设置里将默认输入法设置为fcitx.


# 浏览器

Ubuntu自带浏览器为firefox，但我以前用的一直都是Chrome，书签什么的同步也会方便一点的，所以毫不犹豫的换成了Chrome,`sudo apt-get install google-chrome-stable`

flash插件问题:Chrome用的谷歌自己的flash插件，而firefox默认是没装flash插件的，所以我去abode flash player的[官网](https://get.adobe.com/cn/flashplayer/)手动下载了插件，教程可以看[Ubuntu中文里的介绍](http://wiki.ubuntu.org.cn/Flash)，Ubuntu软件中心装flash插件太坑了！安装的时候等半天根本就不动！


# 词典
看论文及查资料的时候少不了查英文单词，由于在windows里一直用有道词典，所以我先去有道官网看了一下，一看，还真有linux版本，马上下载deb包后用了一下，刚开始觉得还不错，因为有道的鼠标取词（不用划词）这个功能我实在喜欢，虽然取词反应貌似有点慢。但后来我突然发现单词无法**发音**！！查了各种资料还是无法解决，无奈我只能忍痛割爱。

然后我还是用了公认好用的星际译王，不得不说，自己可以自由添加词典，离线也能查单词确实比较强大（*然而现在工作基本都是联网的*）。具体安装及下载词典教程[看这里](http://wiki.ubuntu.org.cn/index.php?title=Stardict&variant=zh-cn)。 

发音我没有装，感觉文件比较大，离线发音没必要，因为我还在Chrome下载了一个插件，我个人觉得非常好用，推荐一下，[ChaZD](https://chrome.google.com/webstore/detail/chazd/nkiipedegbhbjmajlhpegcpcaacbfggp?hl=zh-CN)。

# Matlab
下载matlab估计是最让我蛋疼的事了，在windows直接找个破解版就好了，而找个linux下的破解版真是不容易，虽然好多博客都有贴下载地址，但我试了试总是下载不了，有的就是下到1G左右就不能继续下了，特别折腾，为此我还下了个BT客户端deluge(系统自带的transmission其实也挺好的)。

后来终于找到了一个靠谱的matlab 2010a,用wget后台下载了一天终于下好了，具体教程看[这里](ftp://wcmc.csu.edu.cn/software/%E7%A8%8B%E5%BA%8F%E8%BD%AF%E4%BB%B6/matlab/install%20matlab%20in%20linux.pdf)～

matlab下载好之后还有[中文乱码的问题](http://forum.ubuntu.org.cn/viewtopic.php?t=373776)还有[常见的一些安装问题](https://forum.ubuntu.org.cn/viewtopic.php?f=122&t=443586)这两篇帖子都解决了。


# 办公套件
LibreOffice以及WPC For Linux 都试过,感觉还是不行,格式会乱,所以决定还是office文件老老实实回到windows下编辑,平时自己写文档还是用markdown生成pdf,或者latex都行。

# 工程绘图

由于论文的需要，我需要一个类似MS下visio的工程绘图软件，这里我选的替代品是Dia，这个绘图软件基本能替代visio~安装很简单，也是直接`sudo apt-get install dia`，这里有个地方要注意的是，默认情况下进入dia是intergrated模式，中文显示会有问题，需要做如下修改

    sudo gedit /usr/bin/dia
    #dia-normal --integrated "$@"
    dia-normal "$@"

# 图片处理

Imagemagick！这个图片处理软件真的超棒！可以用import截图，用convert转换图片格式，大小，清晰，可以加滤镜等，非常好用，我直接把它当做默认图片打开方式了。安装同样`sudo apt-get install imagemagick`。

# 视频，音频播放器

Mplayer！这是号称目前这个星球上支持多媒体文件格式最多的软件！哈哈哈，反正目前我电脑里的视频音频格式它都支持，而且gnome版的界面都还不错。详细介绍及安装可以看[这里](http://wiki.ubuntu.com.cn/MPlayer)

# Git
Git的安装很方便，直接`sudo apt-get install git`，因为git本来就是linus在linux下开发的嘛～具体git的使用方法，建议看[廖老师的这篇教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)！

# 终端
我这里直接就用ubuntu自带的gnome-terminal来使用Bash（Bourne again shell），作为一个强迫症患者，我觉得终端自带的紫底白字配色太伤眼睛了，于是上Github寻找好看的配色方案。

刚开始我选了solarized，想把终端配色和vim的配色都弄成solarized。

* 终端的solarized配色文件显示，[github下载](https://github.com/seebi/dircolors-solarized)
* 终端的solarized配色，[github下载](https://github.com/Anthony25/gnome-terminal-colors-solarized)

然而solarized配完后，总觉得看得不舒服，一时切换不过来，强迫症发作的我又去寻找其他配色。最后用了这个[Git](https://github.com/chriskempson/base16-gnome-terminal).
![Ocean Light](/images/my/2.png)

