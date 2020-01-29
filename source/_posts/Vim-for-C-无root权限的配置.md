---
title: Vim for C++无root权限的配置
date: 2020-01-27 17:12:06
tags:  
- Vim
- C++
categories:
- [Linux开发]
---

服务器上一般没有root权限，所以很多工具都需要手动安装。建议先看[有root权限的这篇](https://qindazhu.github.io/2020/01/25/%E6%90%AD%E5%BB%BA%E5%9F%BA%E4%BA%8EVim%E7%9A%84C-%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/)再来看这篇。

## 安装vim8
- `git clone https://github.com/vim/vim.git`
- `cd vim`
- 
  ```
  $ ./configure --with-features=huge \
            --enable-multibyte \
            --enable-rubyinterp=yes \
            --enable-pythoninterp=yes \
            --enable-python3interp=yes \
            --with-python3-config-dir=$(python3-config --configdir) \
            --enable-perlinterp=yes \
            --enable-luainterp=yes \
            --enable-gui=gtk2 \
            --enable-cscope \
            --prefix=your-local-path/vim
  ```
- `make VIMRUNTIMEDIR=your-local-path/vim/share/vim/vim82`
- `make install`
- `you-local-path/vim/bin/vim --version`确认已支持python3
- 在`~/.bash_profile`中加一个alias: `alias myvim='your-local-path/vim/bin/vim -u $HOME/.myvimrc`
- `source ~/.bash_profile`
- `myvim --version`确认alias有效。
- 参考链接
  - https://github.com/ycm-core/YouCompleteMe/wiki/Building-Vim-from-source
  - https://stackoverflow.com/questions/9801360/install-multiple-version-of-vim-and-make-each-use-different-vimrc-file-respec

## 安装Vim-Plug
无区别，直接下载到autoload文件夹就行
```
$ curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## 提示补全
基本一样
- `Plug ‘ycm-core/YouCompleteMe’, { ‘do’: ‘./install.py’ }`
- `Plug ‘rdnetto/YCM-Generator’, { ‘branch’: ‘stable’}`
- `pip3 install jedi --user`
- `git clone --recursive https://github.com/davidhalter/jedi-vim.git ~/.vim/plugged/jedi-vim`
- `Plug 'davidhalter/jedi-vim'`
- 注意，有时候服务器因为`git submodule`的问题，启动vim时YCM会报类似`no variable 'key'`的错误，此时
    - 去YCM插件目录下运行`python3 install.py`，此时可能会提醒你`git submodule`的错误
    - 运行`git submodule update --init --recursive`下载没下载全的一些submodule
    - 再次运行`python3 install.py`
- 有时候服务器上YCM-Generator因为一些原因无法正常运行，此时可以设置`let g:ycm_global_ycm_extra_conf='~/.ycm_extra_conf.py'`直接用自己的设置，可以从[这里](https://github.com/ycm-core/ycmd/blob/master/.ycm_extra_conf.py)下载，然后加入自己的配置

## 语法高亮
无区别，直接安装即可
- `Plug 'sheerun/vim-polyglot'`

## 缩进线
无区别，直接安装即可
- `Plug 'Yggdroot/indentLine'`

## 状态栏
无区别
- ` Plug 'vim-airline/vim-airline'`
- `Plug 'vim-airline/vim-airline-themes'`

## 符号索引
- 安装universal-ctags,祈祷依赖软件你服务器上都有，要不然每个都要手动转就麻烦了，还不如试着去安装
  - git clone https://github.com/universal-ctags/ctags.git
  - 安装
  ```
  $ ./autogen.sh
  $ ./configure --prefix=/where/you/want # defaults to /usr/local
  $ make
  $ make install # may require extra privileges depending on where to install
  ```
  - 在`~/.bash_profile`中加入环境变量并source
- 安装vim-gutentags
  - `Plug 'ludovicchabant/vim-gutentags'`


## 语法检查
- 安装cppcheck
  - `git clone https://github.com/danmar/cppcheck.git`
  - `cd cppcheck`
  - `make MATCHCOMPILER=yes FILESDIR=your-local-path/cppcheck CXXFLAGS="-O2 -DNDEBUG -Wall -Wno-sign-compare -Wno-unused-function"`
  - 上面指定的`FILESDIR`好像没用，还是在当前make目录下生成`cppcheck`文件，`./cppcheck --version`验证安装, `cp cppcheck -r your-local-path/cppcheck`拷贝整个目录到自己的目录，记得是整个，因为还有std.cfg等文件。
  - 在`~/.bash_profile`中加入环境变量指定cppcheck路径
  - `source ~/.bash_profile`
  - 随便写个错误的c++语法验证
- 安装pylint
  - `pip3 install pylint --user`
- 安装ALE
  - `Plug 'dense-analysis/ale'`
  - 配置中加入一句`let g:airline#extensions#ale#enabled = 1`让airline来处理ALE的状态栏


## 搜索文件
无区别，直接安装即可`Plug 'wsdjeg/FlyGrep.vim'`

也可以用[fzf](https://github.com/junegunn/fzf.vim)
- `Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }`
- `Plug 'junegunn/fzf.vim'`


## 源文件头文件切换
无区别， 直接安装`Plug 'tpope/vim-projectionist'`

## 函数列表
- `Plug 'Yggdroot/LeaderF', { 'do': './install.sh' }` 
- 注意LeaderfFunction需要ctags，所以要先安装ctags

## 折叠
无区别， 直接安装`Plug 'tmhedberg/SimpylFold'`

## 文件比较
无区别， 直接安装`Plug 'mhinz/vim-signify'`

## 目录
无区别，直接安装`Plug 'tpope/vim-vinegar'`

## [AsyncRun](https://github.com/skywind3000/asyncrun.vim)
- `Plug 'skywind3000/asyncrun.vim'`
- `:AsyncRun make`
- `:AsyncRun! grep -R <cword> .` 感叹号表示不打开quickfix窗口, <cword>表示当前cursor下的单词
- :AsyncStop[!] 包含感叹号会signal KILL

## 快捷键映射
有时候，服务器上的快捷键映射会被其他软件劫持，比如tmux，你也没有权限改。此时你只能去选择一些没被劫持的键，或者你每次vim启动后重新映射`:so myvimrc`（我的机器上可以）。设置autocmd在我的机器上没用。可以定制一个大概率不会被劫持的快捷键做source，比如`nnoremap bsv :source $MYVIMRC`
- 参考链接
  - https://vi.stackexchange.com/questions/7722/how-to-debug-a-mapping
  - https://vi.stackexchange.com/questions/8075/inoremap-only-works-after-source