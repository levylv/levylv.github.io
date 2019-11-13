---
title: 重装Ubuntu16.04
date: 2017-06-12 17:55:56
tags: Ubuntu
---

之前的Ubuntu14.04用了快两年了，中途经过升级之类各种事，感觉系统里的一些依赖什么都被我折腾坏了，右上角总是有个软件更新冲突提示，所以决定重装Ubuntu16.04。值得一提的是，之前装完Ubuntu14.04写了一篇博客发布在简书上，博客名叫[《开始使用Ubuntu》](http://www.jianshu.com/p/4b9271bba240)(这篇博客也迁移到本站点中了)，至今已被阅读908次，喜欢30次，加入了一些Ubuntu专题，感觉还挺有成就感的。

<!-- more -->

# 分区
有了之前使用Ubuntu14.04的经验，这次我的分区就简单了许多，主分区1,2,3给windows系统，第四个主分区变成拓展分区，拓展为`/boot:500M`(由于经常更新内核，还是需要多一点空间); `/:400G`;`/home:250G`(个人文件夹要放很多文件，所以最好单独分出来);`/swap:8G(大小和内存相似)`。

# VPN
我发现之前从校内网下载的deb包在Ubuntu16.04里无法使用，原因是因为该deb包依赖iproute，然而在我还未联网更新的Ubuntu16.04中没有iproute,iproute2取代了iproute，所以我解压了该deb包，修改了依赖项，并重新打包，然后安装完就ok了。
```
sudo dpkg -X xl2tpd_1.1.12-zju2_am64_new.deb test/ //解压文件
sudo dpkg -e xl2tpd_1.1.12-zju2_am64_new.deb test/ //解压控制文件
修改control文件
sudo dpkg-deb -b test/ new.deb //重新打包
sudo dpkg -i new.deb //安装
```

# 换源
在software&Updates里面可以进行测速，系统会自动选择一个速度最好的源，系统给我选了`http://ubuntu.cn99.com/ubuntu`这个源，保险起见我又添加了一个自己学校的源。
```
# ZJU
deb http://mirrors.zju.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb http://mirrors.zju.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.zju.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
```
换好之后更新源：`sudo apt-get update`

# 搜狗输入法
官网下载deb文件安装即可，`sudo apt-get install -f`解决依赖问题，并且在系统语言设置出选择fcitx,添加sogo pinyin.

# Chrome浏览器
添加第三方源并安装。
```
sudo wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/ 
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt-get update
sudo apt-get install google-chrome-stable
```

# Shadowsocks
不用多说，翻墙是必备的。为了方便起见，我选择安装图形界面的shadowsocks,即shadowsocks-qt5,详细的ss说明可参照[这里](https://github.com/shadowsocks/shadowsocks/wiki),虽然代码已删，但是wiki还在。
```
sudo add-apt-repository ppa:hzwhuang/ss-qt5 //ppa即personal package archives
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```
下载完成后再配置ip地址等等，我买的服务器是包年100元，感觉还凑合。此外，配置好SS后，只是打开了sock5代理端口，如何让chrome用ss代理还是另一码事。

接下来，我们需要在chrome里安装一个插件:SwitchyOmega，插件安装后需要进行配置。首先，新建一个情景模式，然后修改为sock5协议以及配置端口。
![新建SS情景模式](http://upload-images.jianshu.io/upload_images/825093-708ebfa92c818de0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)

然后，我们再在自动切换这个情景模式下进行修改，首先添加一个给GFW墙掉的地址链接，该链接为` https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt`,由[github的一个项目](https://github.com/gfwlist/gfwlist)维护。然后，我们设置该地址里的url,我们用ss代理，其他url全部直接连接，这就相当于一个pac了。
![自动切换情景模式](http://upload-images.jianshu.io/upload_images/825093-90aab2477f7c436f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)

最后保存之后，就可以翻墙了。

# Git
安装很简单，`sudo apt-get install git`,下载好之后，配置一下该电脑下的公私钥。

# Latex
不用多说，写论文必备，当做平时写文章也还行，markdown转pdf我一直都觉得挺麻烦的。。装latex无非就是编译环境和编辑器两方面，编译环境在linux下一般都用texlive，为了方便，我直接安装了全套texlive...整整3G多..`sudo apt-get install texlive-full`。

对于编辑器选择，我直接用的是texmaker,虽然整体来说用得不错，但我还是有点嫌弃它界面有点丑。。我查阅了其他一些流行的编辑器，如sublime,lyX等，最终还是选择了texmaker的fork版texstudio，界面之类的改进了很多，加上之前texmaker习惯大部分都适用，我觉得还是不错的。至于为什么不用vim来编辑latex，我觉得这就像我不用vim编辑markdown一样，我觉得latex及markdown都是需要实时预览，编辑起来才爽的语言，虽然vim也可以搞些插件来预览，但是一方面太麻烦，一方面vim提倡的是解放双手，远离鼠标，一旦有实时预览，双手必然会回归鼠标，我觉得这样就没有必要了，因此对于markdown和latex我都选择了其他编辑器。下图是texstudio界面：
![image.png](http://upload-images.jianshu.io/upload_images/825093-185e4118d7e48ae9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/800)

## 中文支持
其实latex支持中文很简单，只需在头文件处加入`\usepackage{ctex}`或者`\usepackage{xeCJK}`,然后用xelatex编译即可，中文字体也可以通过`\setCJKmainfont{中文字体}`自己设置。

## 字体包问题
我在编译一个文件时用到了`\usapage{uarial}`,但是编译失败，message显示`File 'uarial.sty' not found`，该包没有默认安装，经过google之后，我起初以为是ubuntu没有该uarial字体，于是将windows下的字体都安装到了Ubuntu中，并且还装了文泉译微米黑字体(为了好看)..
```
sudo apt-get install ttf-wqy-microhei 
sudo apt-get install ttf-mscorefonts-installer 
sudo fc-cache -f -v //更新
```
这么做之后并无乱用，因为问题其实是latex缺少包，而非系统缺少字体。。

正确做法是从CTAN下载non free fonts,也就是这些字体包不是免费的(怪不得不默认安装在latex)。。
```
wget -q http://tug.org/fonts/getnonfreefonts/install-getnonfreefonts
sudo texlua ./install-getnonfreefonts
sudo getnonfreefonts --sys -a
```

---

# Matlab R2016a
我分享的iso下载地址及crack破解文件:[百度网盘](http://pan.baidu.com/s/1nuHAUCh)

## 1. 挂载安装
```linux
sudo mkdir /media/matlab
sudo mount -o loop ~/Downloads/R2016a_glnxa64.iso /media/matlab/ //挂载iso到/matlab文件夹
cd /media/matlab
sudo ./install
```

## 2.破解激活
- 安装过程中选择不联网安装,输入产品密钥(crack文件中的FIK).
- 等待安装完成, 默认安装位置为/usr/local/MATLAB/R2016a.
- 安装结束后,打开matlab应用程序.
```linux
cd /usr/local/MATLAB/R2016a/bin/glnxa64/
sudo MATLAB
```
选择离线激活,并添加crack中的Matlab_R2016a_glnxa64.lic.

- 将crack中的另外两个文件复制到matlab安装目录下.
```linux
sudo cp ~/Downloads/crack/libcufft.so.7.5.18 /usr/local/MATLAB/R2016a/bin/glnxa64/
 sudo cp ~/Downloads/crack/libmwservices.so /usr/local/MATLAB/R2016a/bin/glnxa64/
```

## 3. 创建快捷方式
- 由于默认PATH里不包含/usr/local/MATLAB,所以终端直接输入matlab是不行的,可以创建一个软链接
`sudo ln -s /usr/local/MATLAB/R2016a/bin/glnxa64/MATLAB /usr/local/bin/matlab
`
- 为了更加方便,我们可以创建一个桌面快捷方式,在/usr/share/applications/下面创建一个Matlab.desktop,并添加内容如下

        [Desktop Entry]
        Type = Application
        Name = Matlab
        GenericName = Matlab R2016a
        Comment = Matlab R2016a: The Language of the Technical Computing
        Exec = /usr/local/MATLAB/R2016a/bin/glnxa64/MATLAB -desktop //路径需自己修改
        Icon = /usr/local/MATLAB/matlab.png // 网上下载一个快捷方式图标
        StartupNotify = true
        Terminal = false
        Categories = Development;Matlab;
接着加上权限`sudo chmod a+x Matlab.desktop`.
[我的快捷方式图标](http://upload-images.jianshu.io/upload_images/825093-3a8333c910981276.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/120)可供下载.
![Matlab](http://upload-images.jianshu.io/upload_images/825093-3a8333c910981276.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/120)

- 为了避免每次打开matlab后存在权限问题无法读取~/.matab文件的问题，通过`sudo chown [your ubuntu username] -R ~/.matlab`改变权限。

## 4. 字体和快捷键
- 字体美化:进入Matlab，从菜单打开Preferences，打开Fonts页，把右边最下面的复选框Use antialising to smooth desktop fonts选中.
- 中文字体显示问题:可以不用很麻烦,同样打开Preferences->Fonts,挑选一个支持中文的字体就ok了,我选择的是AR PL Ukai CN(楷体)．
- 默认的快捷键是Emacs的，有点不习惯，可以Preferences->Keyboard->Shortcuts->Active settings选Windows Default set.

---
# Hexo
之前在Ubuntu14.04里我用octopress搭建了个人博客，重装之后我原本也是想装回octopress的，但是偶然间发现了hexo，一个更加快速、简洁且高效的博客框架！而且支持octopress的完美迁移，看了用hexo搭建的几个demo之后，我立马就决定这回使用hexo搭建个人博客了。

## 安装与使用说明
hexo的安装和使用可以说相当得简单了，看完[官网的介绍文档](https://hexo.io/zh-cn/docs/index.html)我相信就立马入门了。

## 主题更换
当然了，安装hexo后最重要当然是选一个自己最心仪的主题（其实官网提供的landscape主题其实也还可以。。），经过一番搜索，我选择了github上hexo主题star数排名第一的next主题，附上github[传送门](https://github.com/iissnan/hexo-theme-next)，以及next作者的[demo](http://notes.iissnan.com/)。

Next的主题安装和使用也有着详细的说明文档，附上[传送门](http://theme-next.iissnan.com/getting-started.html),官网的介绍已经很详细了，我也就不在这里赘述了。

最后，我总结一下自己用到的hexo模块：

- 选择Pisces主题（hexo又分为Muse, Mist, Pisces三个主题）。
- 阅读次数统计（LeanCloud）。
- 添加「标签」页面。
- 设置night bright代码高亮主题。
- 侧边栏社交链接添加微博，知乎。
- 开启打赏功能。
- 添加disqus评论系统。
- 添加local search。
- 开启MathJax，这里要注意的是，我在使用分段函数时，分段用的latex代码`\\`只被识别前一个`\`,所以要分段必须使用三个`\`。。

---
# Tensorflow
机器学习将是我的以后工作及学习重心，tensorflow这个平台我必须快速熟悉起来。Tensorflow的安装分为无GPU(只支持CPU)和有GPU的安装，前者安装相当简单，后者的话会很麻烦，还需要对显卡驱动的各种配置。。。由于实验室的工作电脑只是集成显卡而已，所以我就选择了无GPU的安装，当然之后要是有独显了，再研究一下如何支持GPU。

建议使用pip直接进行安装(当然也可以通过docker，Anaconda等第三方环境安装),确保安装了python3及pip3，`sudo apt-get install python3-pip python3-dev`,然后再利用pip3就可以直接安装tensorflow无GPU支持版了，`pip3 install tensorflow`。

## Python安装numpy,scipy,matplotlib库
作为python中重要的科学计算库，numpy，scipy，matplotlib库一定要正确安装。
- NumPy是一个定义了数值数组和矩阵类型和它们的基本运算的语言扩展。 
- SciPy是一种使用NumPy来做高等数学、信号处理、优化、统计和许多其它科学任务的语言扩展。 
- Matplotlib则可能是Python 2D绘图领域使用最广泛的套件。

之前在windows下用pip安装scipy时，总会遇到依赖问题，我只能通过[这篇知乎上的方法](https://www.zhihu.com/question/30188492)，从非官方维护的第三方库安装scipy。然而在ubuntu下，不需要用pip, 直接利用`apt-get`安装，它会将依赖项自动安装，非常简单有效。
```
sudo apt-get install python-numpy
sudo apt-get install python-scipy
sudo apt-get install python-matplotlib
```
## pip换源

由于连接国外官方pypi很慢，我的电脑大概是70kb/s左右的速度，所以最好将pip源更换为国内的镜像源，我使用的是清华大学的pip源。

新建`~/.pip/pip.conf`,创建内容如下:
> 
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

