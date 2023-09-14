---
title: Vim自动补全神器-YouCompleteMe
date: 2017-06-12 18:13:30
categories: [编程开发,Vim]
---

# Vim
Ubuntu自带的vim是vim.tiny版本，很多功能都不全，所以我们需要装一个完整版的，并且我习惯装一个gvim,`sudo apt-get install vim-gtk`。 有意思的是，我发现`apt-get`下面有一个叫`vim+youcompleteme`的版本，我就好奇得装了一下，结果打开vim发现并没有补全功能，但是却装了`ruby，nodejs,ycmd`等几个软件，`ycmd`应该就是补全软件，然而不知道该怎么在vim里使用...所以最终我还是按照github上的说明手动装了`youcompleteme`,这部分留到后文说。

<!-- more -->

## 插件Vundle
Vundle是一个很实用的vim插件，通过它可以方便得管理其他插件，安装很简单。
```git
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

装完之后，需要在`.vimrc`里进行配置，
```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

最终打开vim，输入`：PluginInstall`来安装你在配置文件里写的插件。

我安装了下面几个我常用的插件。
```
" My Plugin here:

" plugin on GitHub repo
Plugin 'Valloric/YouCompleteMe'
Plugin 'luochen1990/rainbow'
Plugin 'fholgado/minibufexpl.vim'
Plugin 'scrooloose/nerdtree'
```

# YouCompleteMe
在我的插件里可以看到我装了YouCompleteMe这个插件，但是光vundle装好还是不够的，我们需要再编译一个能用的引擎。

## 安装必备的编译环境和python环境
```
sudo apt-get install build-essential cmake
sudo apt-get install python-dev python3-dev
```
## 检查vim版本
我们需要检查vim版本以及vim支持的python版本，保证vim版本高于7.4.1578,支持python2或者Python3。我们可以输入命令：`vim --version`来查看，以下是我的输出截图：
![image.png](http://levy-hexo.oss-cn-hangzhou.aliyuncs.com/images/2023-09-14-122449.jpg)
可以看到版本是7.4.1689，不支持python2,但支持python3（貌似Ubuntu16.04开始移除python2的支持了)。

## 编译支持C家族语义补全的YCM
```
cd ~/.vim/bundle/YouCompleteMe
Python3 ./install.py --clang-completer //由于我的vim只支持Python3,所以在前面加python3命令
```
命令执行之后，系统会去下载libclang,因为YCM语义支持是靠clang编译器的，这里需要经过漫长得等待。。如果一切顺利的话，YCM就安装完毕了。

##YCM配置
我的YCM配置如下：
```
"YouCompleteMe
"let g:ycm_path_to_python_interpreter = '/usr/bin/python'
set runtimepath+=~/.vim/bundle/YouCompleteMe
let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM 基于标签引擎
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全
let g:syntastic_ignore_files=[".*\.py$"]
let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全
let g:ycm_complete_in_comments = 1
let g:ycm_confirm_extra_conf = 0
"let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']  " 映射按键, 没有这个会拦截掉tab, 导致其他插件的tab不能用.
"let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全
let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
"let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>" |            " 回车即选中当前项
nnoremap <F5> :YcmCompleter GoToDefinitionElseDeclaration<CR>
"let g:ycm_min_num_of_chars_for_completion=2                 " 从第2个键入字符就开始罗列匹配项
let g:ycm_autoclose_preview_window_after_completion = 1  " 补全后自动关闭preview
```
一个能够进行语法检查，自动补全，并且GoToDefinition的YCM就可以使用了，但是还有一点瑕疵：＃include <iostream>, #include <stdio> vector, 什么的都不能补全，这是因为这些头文件的路径没有被找到，下面的工作就是要让YouCompleteMe找到这些头文件，而且，以后有什么库文件，比如OpenCV，OPenGL什么的，都可以按照这个方法添加。

打开并编辑~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py，这就是我们设定vim默认调用的YCM配置文件，然后可以在flags[*]数组的后面添加你想要的路径，例如: stdio.h等Ｃ语言的头文件包含在/usr/include中，那么您需要添加这样一条
> 
'-isystem',
‘/usr/include’,

如果需要C++的补全，就需要添加：
> 
'-isystem',
‘/usr/include/c++/5’,

需要什么，就添加什么，现在编辑c/c++文件你就发现支持头文件补全了！

# 我的vim配置
根据个人习惯，我的vimrc配置如下：
```
"Wei Lyu
"levy_lv@hotmail.com
"levylv.github.io

"===================="
"        通用        "
"===================="
set nocompatible              " be iMproved, required
filetype plugin indent on
set nobackup "不备份 
set autochdir "自动切换当前目录
set mouse=a

"启动，语法高亮，配色
winpos 500 200   "窗口位置
set lines=30 columns=85  "窗口大小
set guioptions-=T  "不要菜单栏
set laststatus=2   "总是显示状态栏
set hlsearch  "搜索高亮
set ignorecase "搜索忽略大小写
syntax enable
syntax on
set t_Co=256
set cursorline "高亮光标行
set ruler   "显示光标位置状态栏
set number
set guifont=Ubuntu\ Mono\ 13
colorscheme molokai
set clipboard=unnamed "可以用系统剪贴板

"Tab相关
set expandtab "制表符扩展为空格
set tabstop=4 "制表符占用空格数
set softtabstop=4 "将连续数量的空格视为一个制表符
set shiftwidth=4 "格式化时制表符占用空格数
set cindent
set autoindent

"编码相关
set encoding=utf-8
set langmenu=zh_CN.UTF-8
language message zh_CN.UTF-8
set fileencodings=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latin1

"使用CTRL+[hjkl]在窗口间导航"
map <C-c> <C-W>c
map <C-j> <C-W>j
map <C-k> <C-W>k
map <C-h> <C-W>h
map <C-l> <C-W>l

"使用箭头导航buffer"
map <right> :bn<cr>
map <left> :bp<cr>
set autowrite "在切换buffer时自动保存当前的文件

""使用ALT+[jk]来移动行内容
nmap <M-j> mz:m+<cr>`z
nmap <M-k> mz:m-2<cr>`z
vmap <M-j> :m'>+<cr>`<my`>mzgv`yo`z
vmap <M-k> :m'<-2<cr>`>my`<mzgv`yo`z

"根据文件类型做不同处理
function HeaderPython()  "python加头注释
    call setline(1, "#!/usr/bin/env python3")
    call append(1,  "# -*- coding: utf-8 -*-")
    call append(2,  "# mail:levy_lv@hotmail.com")
    call append(3,  "# Lyu Wei @ " . strftime('%Y-%m-%d', localtime()))
    normal G
    normal o
    normal o
endf
autocmd bufnewfile *.py call HeaderPython()

function HeaderBash()  "shell脚本加注释
    call setline(1, "#!/bin/bash")
    call append(1,  "# -*- coding: utf-8 -*-")
    call append(2,  "# mail:levy_lv@hotmail.com")
    call append(3,  "# Lyu Wei @ " . strftime('%Y-%m-%d', localtime()))
    normal G
    normal o
    normal o
endf
autocmd bufnewfile *.sh call HeaderBash()

function HeaderCpp() "C++文件加头文件
    call setline(1, "#include <iostream>")
    call append(1, "using namespace std;")
    normal G
    normal o
    normal o
endf
autocmd bufnewfile *.cpp,*.cc call HeaderCpp()

"C,C++单个文件调试
map <F8> :call Rungdb()<CR>
func! Rungdb()
    exec "w"
    exec "!g++ % -g -o %<"
    exec "!gdb ./%<"
endfunc

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

" plugin on GitHub repo
Plugin 'Valloric/YouCompleteMe'
Plugin 'luochen1990/rainbow'
"Plugin 'majutsushi/tagbar'
Plugin 'fholgado/minibufexpl.vim'
Plugin 'scrooloose/nerdtree'



" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required


"==========================="
"Vundle Plugin Configuration"
"==========================="

"Rainbow
let g:rainbow_active = 1 " 0 if you want to enable it later via: RainbowTogglw

"Tagbar
"nmap <F8> :TagbarToggle<CR>

"YouCompleteMe
"let g:ycm_path_to_python_interpreter = '/usr/bin/python'
set runtimepath+=~/.vim/bundle/YouCompleteMe
let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM 基于标签引擎
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全
let g:syntastic_ignore_files=[".*\.py$"]
let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全
let g:ycm_complete_in_comments = 1
let g:ycm_confirm_extra_conf = 0
"let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']  " 映射按键, 没有这个会拦截掉tab, 导致其他插件的tab不能用.
"let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全
let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
"let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>" |            " 回车即选中当前项
nnoremap <F5> :YcmCompleter GoToDefinitionElseDeclaration<CR>
"let g:ycm_min_num_of_chars_for_completion=2                 " 从第2个键入字符就开始罗列匹配项
let g:ycm_autoclose_preview_window_after_completion = 1  " 补全后自动关闭preview

"minibufexpl
let g:miniBufExplMapWindowNavVim = 1 "可以用<C-h,j,k,l>切换到上下左右的窗口 
let g:miniBufExplMapCTabSwitchBufs = 1 "<C-Tab>,<C-S-Tab>切换
let g:miniBufExplModSelTarget = 1 

"NERDTree
nnoremap <F4> :NERDTreeToggle<CR>


```
