---
title: 搭建基于Vim的C++开发环境
date: 2020-01-25 18:24:56
tags:  
- Vim
- C++
categories:
- [Linux开发]
---
所需功能： 提示补全，定义跳转，关键词搜索，语法高亮，语法检查，缩进折叠，函数展示大纲，文件模板，目录树等等

## 安装Vim 8.0
- 如果有root权限，直接安装
- 如果没有root权限，可以从源码安装https://www.vim.org/git.php
- https://xmfbit.github.io/2018/10/02/vim-you-complete-me/

## 安装[Vim-Plug](https://github.com/junegunn/vim-plug)
vim-plug支持异步安装和按需加载，建议不要再使用[Vundle]。(https://github.com/VundleVim/Vundle.vim)

下载`plug.vim`放到`autoload`文件夹
``` 
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    
```
### 用法
在`~/.vimrc`里配置安装哪些插件
```
" Plugins will be downloaded under the specified directory.
call plug#begin('~/.vim/plugged')

" Declare the list of plugins.
Plug 'tpope/vim-sensible'
Plug 'junegunn/seoul256.vim'

" List ends here. Plugins become visible to Vim after this call.
call plug#end()
```
然后重启vim，运行`:PlugInstall`安装声明的插件，`:PlugStatus`查看安装进度。
`:PlugUpdate`更新插件，`:PlugDiff`跟上一个版本比较，卸载插件的时候，在`~/.vimrc`里删除声明，然后重启vim后运行`:PlugClean`卸载。
具体参考[这里](https://github.com/junegunn/vim-plug/wiki/tutorial)

### 延迟加载
```
" 第一次执行 NERDTreeToggle 命令时，NERD tree 插件才加载
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
" 打开 clojure 类型的文件时，vim-fireplace' 才开始加载
Plug 'tpope/vim-fireplace', { 'for': 'clojure' }
" 同on一样，for 支持多文件类型
Plug 'kovisoft/paredit', { 'for': ['clojure', 'scheme'] }
```

### 一些Tips
比如条件激活等，具体参考[这里](https://github.com/junegunn/vim-plug/wiki/tips)

## 提示补全
这里只安装针对C family和Python的补全支持。

### 插件
- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe)
- [YCM-Generator](https://github.com/rdnetto/YCM-Generator)： 用来解决YouCompleteMe配置文件生成比较麻烦的问题。
- [jedi-vim](https://github.com/davidhalter/jedi-vim)：不一定需要安装，最新版本YCM已经支持python的补全。
- vim omnicomplete: vim自带，作为YCM的补充。可以用`:help ins-completion`查看。

记得确认 Python3, Clang, CMake已经安装，`vim --version`确认vim版本支持python3(输出列表里python3前有`+`号)。
### 安装并配置YouCompleteMe和YCM-Generator
后在`~/.vimrc`中`vim-plug`中的插件列表中加入以下行：
- Plug 'ycm-core/YouCompleteMe', { 'do': './install.py' }
- Plug 'rdnetto/YCM-Generator', { 'branch': 'stable'}
- 运行`:PlugInstall`安装
- 在`/.vimrc`中配置
```
"=========================================
" YCM-Generator 插件配置
"=========================================
" ctrl-I 自动生成 .ycm_extra_conf.py 文件
noremap <C-I> :YcmGenerateConfig -c g++ -v -x c++ -f -b make .<CR>
```
以后只要在项目根目录打开后执行快捷键 Ctrl - i 后就会自动在当前目录生成配置文件
- 参考[这里](https://zhuanlan.zhihu.com/p/33046090)配置YCM
{% codeblock %}
let g:ycm_add_preview_to_completeopt = 0
let g:ycm_show_diagnostics_ui = 0
let g:ycm_server_log_level = 'info'
let g:ycm_min_num_identifier_candidate_chars = 2
let g:ycm_collect_identifiers_from_comments_and_strings = 1
let g:ycm_complete_in_strings=1
let g:ycm_key_invoke_completion = '<c-z>'
set completeopt=menu,menuone

noremap <c-z> <NOP>
         
{% endcodeblock %}
- 注意，如果遇到`ycmd server shut down`的错误，一般重新运行下YCM下的`install.py`就行。
- 如果遇到`python3 missing file header`的问题，`sudo apt-get install python3-dev`安装`python3-dev`就行。

### 安装并配置jedi-vim
- `pip3 install jedi`
- `git clone --recursive https://github.com/davidhalter/jedi-vim.git ~/.vim/plugged/jedi-vim`
- 在`~/.vimrc`里添加`Plug 'davidhalter/jedi-vim'`
 
### 相关快捷键
- **omnicomplete**
	- Ctrl-X Ctrl-L：整行补全
	- Ctrl-X Ctrl-N：当前文件内关键字补全
	- Ctrl-X Ctrl-i：当前文件及其头文件内的关键字
	- Ctrl-X Ctrl-]：tags补全
	- Ctrl-X Ctrl-F：文件名补全
	- Ctrl-X Ctrl-O：全能 (omni) 补全
- **jedi-vim**
	- leader (默认是`\`键) + d：定义跳转，包括语句和 import 模块
	- leader + n：查看用法
	- leader + r：重命名
	- :Pyimport os：打开模块 os
	- Ctrl + space：补全

## 语法高亮
安装[vim-polyglot](https://github.com/sheerun/vim-polyglot)并打开vim的syntax的功能即可
- `~/.vimrc`中加入`Plug 'sheerun/vim-polyglot'`
-  `~/.vimrc`中加入`syntax on`

## 缩进线
- 安装[indentLine](https://github.com/Yggdroot/indentLine)并配置
```
Plug 'Yggdroot/indentLine'
"打开缩进线
let g:indentLine_enabled = 1
let g:indentLine_char='¦'
```
- 设置tab转化为4个空格
```
"将输入的TAB自动展开成空格。开启后要输入TAB，需要Ctrl-V<TAB>
set expandtab
"使用每层缩进的空格数
set shiftwidth=4
"编辑时一个TAB字符占多少个空格的位置
set tabstop=4
"方便在开启了et后使用退格（backspace）键，每次退格将删除X个空格
set softtabstop=4
" 使回格键（backspace）正常处理indent(缩进位置), eol(行结束符), start(段首), 很奇怪 Vim 默认竟然不允许在这些地方使用 backspace
set backspace=indent,eol,start
"开启时，在行首按TAB将加入 shiftwidth 个空格，否则加入 tabstop 个空格
set smarttab
```
- `:IndentLinesToggle`打开缩进

## 语法检查
- 安装[ALE](https://github.com/dense-analysis/ale) `Plug 'dense-analysis/ale'`
- 安装cppcheck和pylint
`sudo apt install cppcheck pylint`
- 配置ALE
```
let g:ale_echo_msg_format = '[%linter%] %code: %%s'
let g:ale_linters = {
    ¦   ¦   \ 'cpp': ['cppcheck'],
    ¦   ¦   \ 'c': ['cppcheck'],
    ¦   ¦   \ 'python': ['pylint'],
    ¦   ¦   \}
" normal 模式下文字改变运行 linter
let g:ale_lint_on_text_changed = 'normal'
" 离开 insert 模式的时候运行 linter
let g:ale_lint_on_insert_leave = 1
let g:ale_c_cppcheck_options = '--enable=all'
let g:ale_cpp_cppcheck_options = '--enable=all'
let g:ale_c_gcc_options = '-Wall -O2 -std=c99'
let g:ale_cpp_gcc_options = '-Wall -O2 -std=c++11'
```
## 搜索文件
- 安装[FlyGrep](https://github.com/wsdjeg/FlyGrep.vim) `Plug 'wsdjeg/FlyGrep.vim'`
- 映射查找快捷键为Ctrl-F `nnoremap <C-F> :FlyGrep<CR>`
- 快捷键
	- Ctrl+F 查找
	- Esc 退出搜索
	- Enter 进入选中的搜索结果
	- Tab 下一个搜索结果
	- Shift+Tab 上一个搜索结果
- 注意，默认FlyGrep只能搜索当前目录，如果要搜索整个project，需要切换到根目录。

## 源文件头文件切换
- 安装[vim-projectionist](https://github.com/tpope/vim-projectionist) `Plug 'tpope/vim-projectionist'`
-  用法: TODO

## 符号索引
python使用jedi-vim的快捷键即可，c++使用ctags.

### 安装[ctags](https://github.com/universal-ctags/ctags)
- `git clone https://github.com/universal-ctags/ctags.git`，也可以用这个[exuberant ctags](http://ctags.sourceforge.net/)安装。
- 安装依赖
```
sudo apt install \
    gcc make \
    pkg-config autoconf automake \
    python3-docutils \
    libseccomp-dev \
    libjansson-dev \
    libyaml-dev \
    libxml2-dev
 ```
- 安装ctags
```
$ ./autogen.sh
$ ./configure --prefix=/where/you/want # defaults to /usr/local
$ make
$ make install # may require extra privileges depending on where to install
```
- 创建文件`~/.ctags.d/my.ctags`文件用来配置，比如可以加入配置行`--output-format=e-ctags`以保持跟exuberant ctags兼容。 

### 安装[vim-gutentags](https://github.com/ludovicchabant/vim-gutentags)
- 安装`Plug 'ludovicchabant/vim-gutentags'`
- 配置
{% codeblock %}
" gutentags 搜索工程目录的标志，碰到这些文件/目录名就停止向上一级目录递归
let g:gutentags_project_root = ['.root', '.svn', '.git', '.hg', '.project']

" 所生成的数据文件的名称
let g:gutentags_ctags_tagfile = 'tags'

" 配置 ctags 的参数
let g:gutentags_ctags_extra_args = ['--fields=+niazS', '--extra=+q']
let g:gutentags_ctags_extra_args += ['--c++-kinds=+px']
let g:gutentags_ctags_extra_args += ['--c-kinds=+px']

{% endcodeblock %}

### 用法
- 命令
	- 命令行下`ctags -R .`或者vim中`:!ctags -R .`生成tags文件,-f 参数可以指定生成文件的路径。 
	- CTRL-]跳转定义
	- CTRL-O跳回
	- CTRL-W ] 新窗口打开定义
	- :ts tag 搜索tag
	- :tn 跳转到下一个定义
	- :tp 跳转到上一个定义
	- :ts 列出所有定义
	- :tf 跳转到第一个定义
	- :tl 跳转到最后一个定义
	- :tselect 选择跳转到哪一个
	- CTRL-t 回退到上一个tag stack
- 参考链接
	- https://andrew.stwrt.ca/posts/vim-ctags/

## 状态栏
安装[vim-airline](https://github.com/vim-airline/vim-airline)

## 函数列表
- 安装[LeaderF](https://github.com/Yggdroot/LeaderF) 

```
Plug 'Yggdroot/LeaderF', { 'do': './install.sh' }
noremap <C-P> :LeaderfFile<CR>
noremap <C-S-O> :LeaderfFunction!<CR>
noremap <C-N> :LeaderfMru<CR>
```
- 配置

```
let g:Lf_ShowRelativePath = 0
let g:Lf_HideHelp = 1
let g:Lf_PreviewResult = {'Function':0, 'Colorscheme':1}

let g:Lf_NormalMap = {
	\ "File":   [["<ESC>", ':exec g:Lf_py "fileExplManager.quit()"<CR>']],
	\ "Buffer": [["<ESC>", ':exec g:Lf_py "bufExplManager.quit()"<CR>']],
	\ "Mru":    [["<ESC>", ':exec g:Lf_py "mruExplManager.quit()"<CR>']],
	\ "Tag":    [["<ESC>", ':exec g:Lf_py "tagExplManager.quit()"<CR>']],
	\ "Function":    [["<ESC>", ':exec g:Lf_py "functionExplManager.quit()"<CR>']],
	\ "Colorscheme":    [["<ESC>", ':exec g:Lf_py "colorschemeExplManager.quit()"<CR>']],
	\ }
```
### 用法
- `:LeaderfCommand`进入后按`i`可以进入模糊匹配
-  `Ctrl-N`搜索最近打开的文件

## 折叠
安装[SimpylFold](https://github.com/tmhedberg/SimpylFold) `Plug 'tmhedberg/SimpylFold'`
- visual mode选中，`z-F`折叠，然后`zc`关闭折叠，`zo`打开折叠

## 文件比较
安装[vim-signify](https://github.com/mhinz/vim-signify)` Plug 'mhinz/vim-signify'`
- `:SignifyDiff` 可以分屏对比diff

## 目录
- 安装[vim-vinegar](https://github.com/tpope/vim-vinegar) `Plug 'tpope/vim-vinegar'`
-  使用，基本同netrw
	- :Explore - opens netrw in the current window
	- :Sexplore - opens netrw in a horizontal split
	- :Vexplore - opens netrw in a vertical split
	- 按`-`往上走，按`Enter`往下走

## Tips
- Vim中ALT键都留给用户了，所以多用ALT来映射快捷键
- 额外快捷键配置 https://github.com/tpope/vim-unimpaired

## Links
- https://www.zhihu.com/question/47691414
- https://juejin.im/post/5cdc396af265da03576ee968
- https://www.zhihu.com/question/31934850
- http://vimcasts.org/blog/2013/01/oil-and-vinegar-split-windows-and-project-drawer/