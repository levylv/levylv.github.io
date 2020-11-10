---
title: 从零搭建Mac开发环境
date: 2020-06-20
categories: [操作系统,Mac]
---

前阵子电脑mac进水，搞了台备机，重新安装了各类软件搭建开发环境，特此记录...以防万一...

###  软件包

1. Chrome

2. 搜狗输入法

3. pycharm（2020.1）

   - 模板：Editor -> File and code Templates  -> Python Script：

     ```python
      #!/usr/bin/env python
      # -*- coding: utf-8 -*-
      # @Author: levylv
      # @Date  : ${DATE}
     ```

   - 激活 方法同idea

4. Idea （2020.1）

   - 模板：Editor -> File and code Templates  -> File Header:

     ```java
     /**
      * Created by levylv on ${YEAR}/${MONTH}/${DAY}.
      */
     ```

   - 激活 2020.1.2破解 https://www.jianshu.com/p/46f00b2ce3ce

   - 插件 leetcode-editor https://github.com/shuzijun/leetcode-question , lombok 官网下载

   - 插件 easycode

5. 钉钉

6. dchat

7. 微信

8. 网易云音乐

9. typora

   - ipic图床



### 开发环境：

1. Anaconda（4.8.2）

   - 新版conda自己会配置环境变量，以及激活base环境

2. jdk 8

   - 配置java_home：~/.bash_profile。完整的~/.bash_profile配置看后面

3. iterm2

   - 颜色配置：~/.bash_profile

4. homebrew https://zhuanlan.zhihu.com/p/90508170

5. macvim

   - brew install macvim

   - 配置文件~/.vimrc，我的祖传配置：

     ```sh
     "Wei Lyu
     "levy_lv@hotmail.com
     "levylv.github.io
     
     "===================="
     "        通用        "
     "===================="
     set nocompatible              " be iMproved 
     filetype plugin indent on
     set nobackup "不备份 
     set autochdir "自动切换当前目录
     
     "启动，语法高亮，配色
     winpos 400 200
     set lines=100 columns=150
     set laststatus=2   "总是显示状态栏
     set hlsearch  "搜索高亮
     set ignorecase "搜索忽略大小写
     syntax enable
     syntax on
     set t_Co=256
     set cursorline "高亮光标行
     set ruler   "显示光标位置状态栏
     set number
     set guifont=Monaco:h15
     colorscheme molokai
     let g:molokai_original = 1
     let g:rehash256 = 1
     
     
     "Tab相关
     set tabstop=4 "制表符占用空格数
     set softtabstop=4 "将连续数量的空格视为一个制表符
     set shiftwidth=4 "格式化时制表符占用空格数
     set expandtab "制表符扩展为空格
     set cindent
     set autoindent
     
     "编码相关
     set encoding=utf-8
     set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1
     
     "使用箭头导航buffer"
     "map <right> :bn<cr>
     "map <left> :bp<cr>
     "set autowrite "在切换buffer时自动保存当前的文件
     
     
     "===================="
     "Vundle Configuration"
     "===================="
     filetype off                  " required
     
     " set the runtime path to include Vundle and initialize
     set rtp+=~/.vim/bundle/Vundle.vim
     call vundle#begin()
     " alternatively, pass a path where Vundle should install plugins
     "call vundle#begin('~/some/path/here')
     
     " let Vundle manage Vundle, required
     Plugin 'VundleVim/Vundle.vim'
     
     " My Plugin here:
     " Plugin 'Valloric/YouCompleteMe'
     " Plugin 'majutsushi/tagbar'
     " Plugin 'fholgado/minibufexpl.vim'
     Plugin 'scrooloose/nerdtree'
     
     
     
     " All of your Plugins must be added before the following line
     call vundle#end()            " required
     filetype plugin indent on    " required
     
     
     "==========================="
     "Vundle Plugin Configuration"
     "==========================="
     
     "minibufexpl
     "let g:miniBufExplMapWindowNavVim = 1 "可以用<C-h,j,k,l>切换到上下左右的窗口 
     "let g:miniBufExplMapCTabSwitchBufs = 1 "<C-Tab>,<C-S-Tab>切换
     "let g:miniBufExplModSelTarget = 1 
     
     "NERDTree
     nnoremap <Tab> :NERDTreeToggle<CR>
     
     ```

   - 主题molokai

   - vundle插件

6. Maven（3.6.3） https://www.runoob.com/maven/maven-setup.html

7. ClashX 翻墙：https://github.com/yichengchen/clashX/releases

8. git

   - ~/.gitconfig配置，祖传alias配置：

     ```shell
     [user]
     	name = lvwei
     	email = ***
     
     [alias]
     	a 	 = !git add . && git status
     	aa       = !git add . && git add -u . && git status
     	ac       = !git add . && git commit
     	acm      = !git add . && git commit -m
     	alias    = !git config --list | grep 'alias'
     	au       = !git add -u . && git status
     	c        = commit
     	ca       = commit --amend
     	cm       = commit -m
     	d        = diff
     	l        = log --graph --all --pretty=format:'%C(yellow)%h%C(cyan)%d%Creset %s %C(white)- %an, %ar%Creset'
     	lg       = log --color --graph --pretty=format:'%C(bold white)%h%Creset -%C(bold green)%d%Creset %s %C(bold green)(%cr)%Creset %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
     	ll       = log --stat --abbrev-commit
     	llg      = log --color --graph --pretty=format:'%C(bold white)%H %d%Creset%n%s%n%+b%C(bold blue)%an <%ae>%Creset %C(bold green)%cr (%ci)' --abbrev-commit
     	master   = checkout master
     	s        = status
     	spull    = svn rebase
     	spush    = svn dcommit
     
     [credential]
     	helper = store
     [core]
     	excludesfile = /Users/didi/.gitignore_global
     ```

   - .gitignore_global配置，祖传全局ignore配置：

     ```shell
     # Compiled source #
     ###################
     *.com
     *.class
     *.dll
     *.exe
     *.o
     *.so
      
     # Packages #
     ############
     # it's better to unpack these files and commit the raw source
     # git has its own built in compression methods
     *.7z
     *.dmg
     *.gz
     *.iso
     *.jar
     *.rar
     *.tar
     *.zip
     # Logs and databases #
     ######################
     *.log
     *.sql
     *.sqlite
     # OS generated files #
     ######################
     .DS_Store
     .DS_Store?
     .idea
     ._*
     .Spotlight-V100
     .Trashes
     Icon?
     ehthumbs.db
     Thumbs.db
     ```

     

9. ~/.bash_profile配置:

   ```shell
   # alias
   alias ll='ls -lF'
   alias vi='mvim'
   
   # iterm2
   export CLICOLOR=1
   export LSCOLOR=Gxfxaxdxcxegedabagacad
   export PS1='\[\e[01;33m\][\[\e[01;36m\]\u\[\e[01;33m\]@\[\e[01;35m\]\h\[\e[01;33m\]] \[\e[01;36m\]\w \[\e[01;32m\]\$\[\033[00m\] ' #显示全部路径名
   
   # JAVAHOME
   export JAVA_HOME=$(/usr/libexec/java_home)
   export PATH=$JAVA_HOME/bin:$PATH
   
   # Maven
   export MAVEN_HOME=/usr/local/apache-maven-3.6.3
   export PATH=$PATH:$MAVEN_HOME/bin
   
   # bash shell
   export BASH_SILENCE_DEPRECATION_WARNING=1
   
   # >>> conda initialize >>>
   # !! Contents within this block are managed by 'conda init' !!
   __conda_setup="$('/opt/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
   if [ $? -eq 0 ]; then
       eval "$__conda_setup"
   else
       if [ -f "/opt/anaconda3/etc/profile.d/conda.sh" ]; then
           . "/opt/anaconda3/etc/profile.d/conda.sh"
       else
           export PATH="/opt/anaconda3/bin:$PATH"
       fi
   fi
   unset __conda_setup
   # <<< conda initialize <<<
   export PATH=/opt/anaconda3/bin:$PATH
   
   ```

10. tmux配置, ~/.tmux.conf

    ```shell
    setw -g mode-keys vi
    new-session -n $HOST
    set-option -g status-interval 1
    set-option -g automatic-rename on
    #set-option -g automatic-rename-format '#(basename "#{pane_current_path}")'
    
    #select last window
    bind-key C-l select-window -l
    
    #up
    bind-key k select-pane -U
    #down
    bind-key j select-pane -D
    #left
    bind-key h select-pane -L
    #right
    bind-key l select-pane -R
    
    # resize pane
    bind -r ^k resizep -U 1 # upward (prefix Ctrl+k)
    bind -r ^j resizep -D 1 # downward (prefix Ctrl+j)
    bind -r ^h resizep -L 1 # to the left (prefix Ctrl+h)
    bind -r ^l resizep -R 1 # to the right (prefix Ctrl+l)
    bind % split-window -h -c "#{pane_current_path}"
    ```

    