---
title: vim配置
date: 2016-06-19 22:43:00
tags: vim
categories: tools
keywords: vim
---
目录
1. [VIM 简单配置](#vim-简要配置)
2. [VIM 快捷键不同的映射方式](#VIM-快捷键不同的映射方式)
3. [VIM buffer](#VIM-buffer)
4. [VIM .vimrc leader 的解释与使用](#VIM-vimrc-leader)


#### vim 简要配置
主要包括vim一些常用的配置，如文件编码、语法高亮、字体设置、主题设置、代码缩进、tab键设置等等。
<!--more-->

先安装Vundle
``git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim``
再安装ctags-etags
然后将下面的配置文件放到.vimrc文件中

``` vim
set nocompatible              " be iMproved, required
filetype off                  " required
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')
" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'Valloric/YouCompleteMe'
Plugin 'tpope/vim-fugitive'
Plugin 'L9'
Plugin 'wincent/command-t'
Plugin 'vim-scripts/The-NERD-tree'
Plugin 'vim-scripts/taglist.vim'
Plugin 'jlanzarotta/bufexplorer'
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
call vundle#end()            " required
filetype plugin indent on    " required
"Bundle 'Valloric/YouCompleteMe'
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
"------------------------------------------------------------------------------
set encoding=utf-8
set fileencodings=ucs-bom,utf-8,gb18030,euc-cn,cp936,gbk,gb2312,euc-tw,big5,euc-jp,euc-kr,latin1
set fileformats=unix,dos
syntax on "自动语法高亮
colorscheme koehler
" taglist.vim
let g:Tlist_Auto_Update=1
let g:Tlist_Process_File_Always=1
let g:Tlist_Exit_OnlyWindow=1
let g:Tlist_Show_One_File=1
" let g:Tlist_WinWidth=25
" let g:Tlist_Enable_Fold_Column=0
let g:Tlist_Auto_Highlight_Tag=1
" NERDTree.vim
let g:NERDTreeWinPos="left"
let g:NERDTreeWinSize=35
let g:NERDTreeShowLineNumbers=1
let g:NERDTreeQuitOnOpen=1
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'
let g:bufExplorerDefaultHelp=1       " Show default help. 
let g:bufExplorerShowRelativePath=1  " Show relative paths.
let g:bufExplorerSortBy='mru'        " Sort by most recently used.
"let g:bufExplorerSplitRight=0        " Split left.
"let g:bufExplorerSplitVertical=1     " Split vertically.
let g:bufExplorerSplitVertSize = 30  " Split width
let g:bufExplorerUseCurrentWindow=1  " Open in new window.

let g:ycm_global_ycm_extra_conf='/home/teamvpn/work/gitlab-yxzc/vpn-openvpn/OPENVPN_SERVER/source_code/.ycm_extra_conf.py'
let g:ycm_seed_identifiers_with_syntax = 1  "C/C++关键字自动补全
let g:ycm_python_binary_path='/usr/bin/python'
let g:ycm_collect_identifiers_from_tags_files=1
set completeopt-=preview  "补全内容不以分割子窗口形式出现，只显示补全列表
let g:ycm_cache_omnifunc=0 "禁止缓存匹配项，每次都重新生成匹配
let g:ycm_key_invoke_completion='<M-;>' "修改对C函数的补全快捷键,默认是CTRL+space,修改为ALT + ;
"设置转到定义的快捷键,这个功能非常赞
nnoremap <leader>aa :YcmCompleter GoTo<CR>
nnoremap <leader>ab :YcmCompleter GoToDefinition<CR>
nnoremap <leader>ac :YcmCompleter GoToDeclaration<CR>
nnoremap <leader>ad :YcmCompleter GoToInclude<CR>
nnoremap <leader>ae :YcmCompleter GetType<CR>
nnoremap <leader>af :YcmCompleter GetDoc<CR>

nmap  <F1> :TlistToggle<cr>
nmap  <F2> :NERDTreeToggle<cr>
"nmap  <F3> :!make clean<cr>
nmap  <F3> :!ctags -R <cr>
nmap  <F4> :!make<cr>
set autowrite 
set autoread
set showcmd
set showmode
set title
set foldenable      " 允许折叠  
set foldmethod=manual   " 手动折叠  
set si          " smartindent 类似于语法匹配缩进
set autoindent
set cindent
set smarttab        " 在行和段开始处使用制表符
set shiftwidth=4    " 设定 << 和 >> 命令移动时的宽度为 4
set softtabstop=4   " 使得按退格键时可以一次删掉 4 个空格
set tabstop=4       " 设定 tab 长度为 4
set expandtab       " 输入等同一个tab长度的空格
"set noexpandtab      " 不要用空格代替制表符
set wrap            " 自动换行
set backspace=indent,eol,start " 不设定的话在插入状态无法用退格键和 Delete 键删除回车符
set ruler           " 打开状态栏标尺
set nobackup        " 覆盖文件时不备份
set backupcopy=yes  " 设置备份时的行为为覆盖
set ignorecase smartcase " 搜索时忽略大小写，但在有一个或以上大写字母时仍保持对大小写敏感
set nowrapscan           " 禁止在搜索到文件两端时重新搜索
set incsearch            " 输入搜索内容时就显示搜索结果
set hlsearch             " 搜索时高亮显示被找到的文本
"set noerrorbells         " 关闭错误信息响铃
"set novisualbell         " 关闭使用可视响铃代替呼叫
"set t_vb=                " 置空错误铃声的终端代码
set showmatch " 插入括号时，短暂地跳转到匹配的对应括号
set matchtime=2 " 短暂跳转到匹配括号的时间
set laststatus=2 " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%05l,%05v][%p%%]\ [LINE=%L]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}
set cmdheight=1 " 设定命令行的行数为 1
set magic " 设置魔术
set hidden " 允许在有未保存的修改时切换缓冲区，此时的修改由 vim 负责保存
set guioptions-=T " 隐藏工具栏
set guioptions-=m " 隐藏菜单栏

autocmd FileType make setlocal noexpandtab
"autocmd FileType * set tabstop=2|set shiftwidth=2|set noexpandtab
"autocmd FileType python set tabstop=4|set shiftwidth=4|set expandtab
"autocmd BufEnter *.py set ai sw=4 ts=4 sta et fo=croql
```

将配置放到.vimrc后，保存关闭，然后再重新打开，再执行``:PluginInstall``,让Vundle将设置好的插件下载并安装。 最后再将``.vim/bundle/bufexplorer/plugin/bufexplorer.vim``中里面的be改成bb，习惯用\bb

将Youcompleteme的配置文件放到每个工程的根目录中，每个工程的代码和选项都不太可能是一样的。尤其是include的header文件，每个工程的include路径都需要配置到.ycm_extra_conf文件中，所以每个.ycm_extra_conf文件里面的内容都不一样。[youcomplete config](#zz01), [vim 配合youcompleteme的启动脚本](#zz02) 当把配置文件和vim启动脚本放好后，就可以sh vim.sh以这种方式启动vim了，不过应该有更好的启动方式的。

#### VIM 快捷键不同的映射方式
inoremap  在插入(insert)模式下生效
vnoremap  在visual模式下生效
nnoremap  在normal模式下(狂按esc后的模式)生效

[参考链接 vim中为什么有那么多map？nnoremap, vnoremap](https://www.zhihu.com/question/20741941/answer/16080645)
[参考链接 vim的几种模式和按键映射](http://haoxiang.org/2011/09/vim-modes-and-mappin/)

#### VIM vimrc leader
The Leader key is mapped to ``\`` by default. So if you have a map of ``Leader t``, you can execute it by default with ``\``+``t``.

[参考链接 vim leader](http://stackoverflow.com/questions/1764263/what-is-the-leader-in-a-vimrc-file)

#### VIM buffer
1. vim还没有启动的时候： 在终端里输入 ``vim file1 file2 ... fileN``便可以打开所有想要打开的文件
2. vim已经启动,输入 :edit file 可以再打开一个文件，并且此时vim里会显示出file文件的内容。
3. 同时显示多个文件： 横向:split 纵向:vsplit

在buffer之间切换
1.文件间切换 ``Ctrl+6``下一个文件 ``:bn``—下一个文件,``:bp``—上一个文件 对于用(v)split在多个窗格中打开的文件，这种方法只会在当前窗格中切换不同的文件。
2.在窗格间切换的方法 ``Ctrl+w+方向键``—切换到前／下／上／后一个窗格;``Ctrl+w+h/j/k/l``—同上;``Ctrl+ww``—依次向后切换到下一个窗格中


<span id="zz01">An example for youcompleteme config </span>
``` py
# This file is NOT licensed under the GPLv3, which is the license for the rest
# of YouCompleteMe.
#
# Here's the license text for this file:
#
# This is free and unencumbered software released into the public domain.
#
# Anyone is free to copy, modify, publish, use, compile, sell, or
# distribute this software, either in source code form or as a compiled
# binary, for any purpose, commercial or non-commercial, and by any
# means.
#
# In jurisdictions that recognize copyright laws, the author or authors
# of this software dedicate any and all copyright interest in the
# software to the public domain. We make this dedication for the benefit
# of the public at large and to the detriment of our heirs and
# successors. We intend this dedication to be an overt act of
# relinquishment in perpetuity of all present and future rights to this
# software under copyright law.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
# IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
# OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
# ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
# OTHER DEALINGS IN THE SOFTWARE.
#
# For more information, please refer to <http://unlicense.org/>

import os
import ycm_core

# These are the compilation flags that will be used in case there's no
# compilation database set (by default, one is not set).
# CHANGE THIS LIST OF FLAGS. YES, THIS IS THE DROID YOU HAVE BEEN LOOKING FOR.
flags = [
'-Wall',
'-Wextra',
'-Werror',
'-Wc++98-compat',
'-Wno-long-long',
'-Wno-variadic-macros',
'-fexceptions',
'-DNDEBUG',
# You 100% do NOT need -DUSE_CLANG_COMPLETER in your flags; only the YCM
# source code needs it.
# THIS IS IMPORTANT! Without a "-std=<something>" flag, clang won't know which
# language to use when compiling headers. So it will guess. Badly. So C++
# headers will be compiled as C headers. You don't want that so ALWAYS specify
# a "-std=<something>".
# For a C project, you would set this to something like 'c99' instead of
# 'c++11'.
'-std=c99',
#'-std=c++11',

# ...and the same thing goes for the magic -x option which specifies the
# language that the files to be compiled are written in. This is mostly
# relevant for c++ headers.
# For a C project, you would set this to 'c' instead of 'c++'.
'-x',
'c',
'-isystem',
'/usr/include',
'-isystem',
'/usr/local/include',
'-isystem',
'/home/lvzhx/work/allljx/include',
'-isystem',
'/home/lvzhx/work/allljx/thirdparty/include',
]


# Set this to the absolute path to the folder (NOT the file!) containing the
# compile_commands.json file to use that instead of 'flags'. See here for
# more details: http://clang.llvm.org/docs/JSONCompilationDatabase.html
#
# You can get CMake to generate this file for you by adding:
#   set( CMAKE_EXPORT_COMPILE_COMMANDS 1 )
# to your CMakeLists.txt file.
#
# Most projects will NOT need to set this to anything; you can just change the
# 'flags' list of compilation flags. Notice that YCM itself uses that approach.
compilation_database_folder = ''

if os.path.exists( compilation_database_folder ):
  database = ycm_core.CompilationDatabase( compilation_database_folder )
else:
  database = None

SOURCE_EXTENSIONS = [ '.cpp', '.cxx', '.cc', '.c', '.m', '.mm' ]

def DirectoryOfThisScript():
  return os.path.dirname( os.path.abspath( __file__ ) )


def MakeRelativePathsInFlagsAbsolute( flags, working_directory ):
  if not working_directory:
    return list( flags )
  new_flags = []
  make_next_absolute = False
  path_flags = [ '-isystem', '-I', '-iquote', '--sysroot=' ]
  for flag in flags:
    new_flag = flag

    if make_next_absolute:
      make_next_absolute = False
      if not flag.startswith( '/' ):
        new_flag = os.path.join( working_directory, flag )

    for path_flag in path_flags:
      if flag == path_flag:
        make_next_absolute = True
        break

      if flag.startswith( path_flag ):
        path = flag[ len( path_flag ): ]
        new_flag = path_flag + os.path.join( working_directory, path )
        break

    if new_flag:
      new_flags.append( new_flag )
  return new_flags


def IsHeaderFile( filename ):
  extension = os.path.splitext( filename )[ 1 ]
  return extension in [ '.h', '.hxx', '.hpp', '.hh' ]


def GetCompilationInfoForFile( filename ):
  # The compilation_commands.json file generated by CMake does not have entries
  # for header files. So we do our best by asking the db for flags for a
  # corresponding source file, if any. If one exists, the flags for that file
  # should be good enough.
  if IsHeaderFile( filename ):
    basename = os.path.splitext( filename )[ 0 ]
    for extension in SOURCE_EXTENSIONS:
      replacement_file = basename + extension
      if os.path.exists( replacement_file ):
        compilation_info = database.GetCompilationInfoForFile(
          replacement_file )
        if compilation_info.compiler_flags_:
          return compilation_info
    return None
  return database.GetCompilationInfoForFile( filename )


def FlagsForFile( filename, **kwargs ):
  if database:
    # Bear in mind that compilation_info.compiler_flags_ does NOT return a
    # python list, but a "list-like" StringVec object
    compilation_info = GetCompilationInfoForFile( filename )
    if not compilation_info:
      return None

    final_flags = MakeRelativePathsInFlagsAbsolute(
      compilation_info.compiler_flags_,
      compilation_info.compiler_working_dir_ )

    # NOTE: This is just for YouCompleteMe; it's highly likely that your project
    # does NOT need to remove the stdlib flag. DO NOT USE THIS IN YOUR
    # ycm_extra_conf IF YOU'RE NOT 100% SURE YOU NEED IT.
    try:
      final_flags.remove( '-stdlib=libc++' )
    except ValueError:
      pass
  else:
    relative_to = DirectoryOfThisScript()
    #relative_to ='/usr/include'
    final_flags = MakeRelativePathsInFlagsAbsolute( flags, relative_to )

  return { 'flags': final_flags }
```

<span id="zz02">vim 配合Youcompleteme的启动脚本 </span>
``` sh
#!/bin/sh

if [ -e .ycm_extra_conf.py ];then
        sed -i '/let g:ycm_global_ycm_extra_conf=/d' $HOME/.vimrc
        sed -i "/let g:ycm_seed_identifiers_with/i let g:ycm_global_ycm_extra_conf='"$PWD"/.ycm_extra_conf.py'" $HOME/.vimrc
        vim 
else
        echo " .ycm_extra_conf.py not found!"
fi
```
