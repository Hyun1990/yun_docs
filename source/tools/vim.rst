Vim
=============

安装
--------

``$ sudo apt-get install vim // Ubuntu``

新手指南
--------

``$ vimtutor //vim 教程``

移动光标
~~~~~~~~

| ``hjkl      左下上右``
|
| ``2w        移动到下两个单词词首``
| ``3e        移动到下3个单词的末尾``
| ``b         移动到上一个单词的词首``
| ``ge        移动到上一个单词结尾``
|
| ``0         移动到行首`` 
| ``$         移动到当前行的末尾``
| ``gg        文件第一行``
| ``G         文件最后一行`` 
| ``行号+G    指定行``
| ``<ctrl>+o  跳转回之前的位置``
| ``<ctrl>+i  返回跳转之前的位置``

退出
~~~~~

| ``<esc>     进入命令行模式``
| ``:q!       不保存退出``
| ``:wq       保存后退出``

删除
~~~~~

| ``x 	      删除当前字符``
| ``dw        删除当前单词``
| ``ndw       删除以当前单词开始的n个单词``
| ``d$        删除至当前行尾``
| ``d0        删除到行首``
| ``dd        删除整行``
| ``ndd       删除从当前行开始的n行``
| ``:10,20d   删除10～20行``
| ``.,$d      删除当前行到末尾``
| ``d4        删除到第4个语句的结尾位置``
| ``db        删除到某个单词开始位置``
| ``dW        删除到以某个空格为分割符的单词结尾位置``
| ``dB        删除到以某个空格为分割符的单词开始位置``
| ``d}        删除到某个段落结尾位置``
| ``d{        删除到某个段落开始位置``
| ``d/text    删除从文本中出现text中所指定字样的位置``

修改
~~~~~

| ``i         当前光标插入文本``
| ``I         在当前行开头处插入数据``
| ``a         光标后位置添加``
| ``A         当前行末尾添加``
| ``o         在当前行下面插入一行``
| ``O         在当前行上面插入一行``
| ``r+字符    替换当前字符``

撤销
~~~~~

| ``u         撤销``
| ``<ctrl>+r  取消撤销``

复制粘贴剪切
~~~~~~~~~~~~~

| ``v         进入可视模式``
| ``y         复制``
| ``p         粘贴``
| ``yy        复制当前行``
| ``dd        剪切当前行``

文件
~~~~~

| ``:e!               强制刷新该文件``
| ``<ctrl>+g          显示当前行及文件信息``
| ``vim f1 f2         打开多个文件``
| ``:n                切换到下一个文件``
| ``:wn               保存再切换``
| ``:N                切换到上一个文件``
| ``:wN               保存再切换``
| ``:.                查看当前行``
| ``:n <Enter> :600,p 到下一个文件，从600行开始粘贴``
| ``wq                保存退出``
| ``:sp filename      在新的缓冲区打开一个文件并分割窗口``
| ``:nb               删除一个缓冲区``

查找
~~~~~

| ``/text     正向查找(n:继续查找，N:向反方向继续查找)``
| ``?text     逆向查找``
| ``%         查找配对的{,[,(``
| ``:set ic   忽略大小写``
| ``:set noic 取消忽略大小写``
| ``:set hls  匹配项高亮显示``
| ``:set is   显示部分匹配``

替换
~~~~~

| ``:s/old/new       替换该行第一个匹配串``
| ``:s/old/new/g     替换全行的匹配串``
| ``:%s/old/new/      替换每一行的第一个匹配串``
| ``:%s/old/new/g     替换整个文件的匹配串``
| ``:n,$/old/new     替换第n行到最后一行第一个匹配串``
| ``:n,$/old/new/g   替换第n行到最后一行所有匹配串``

折叠
~~~~~

| ``zc        折叠``
| ``zC        折叠所有嵌套``
| ``zo        展开折叠``
| ``zO        展开所有折叠嵌套``

执行外部命令
~~~~~~~~~~~~

| ``:!shell``

字体
~~~~

| ``<ctrl> -        减小``
| ``<ctrl> shift +  放大``
| ``<ctrl> 0        还原``

分屏
~~~~~

| ``ctrl+w s          上下分割窗口``
| ``ctrl+w v          左右分割窗口``
| ``ctrl+w w          切换窗口``
| ``ctrl+w q          退出窗口``
| ``ctrl+w h/j/k/l    移动光标``
| ``ctrl+w r          移动当前窗口的布局位置``
| ``ctrl+w H/J/K/L    移动分屏``
| ``ctrl=w =/+/-      修改屏幕尺寸``

基本配置
--------
.vimrc 是 Vim的配置文件，分为全局配置文件和用户配置文件，建议修改只修改用户的配置文件

全局配置文件在/etc/vim/vimrc
用户配置文件在/home/hyun/.vimrc 中

::

	" 开启文件类型侦测
	filetype on
	" 根据侦测到的不同类型加载对应的插件
	filetype plugin on
	" 让配置变更立即生效
	autocmd BufWritePost $MYVIMRC source $MYVIMRC
	" 自身命令行模式智能补全

快捷键
~~~~~~~

把vim(非插件)常用操作设定成快捷键，提升效率

::

	" 定义快捷键的前缀
	let mapleader=";"
	" 定义快捷键关闭当前分割窗口
	nmap <leader>q :q<CR>
	" 定义快捷键保存当前窗口内容
	nmap <leader>w :w<CR>
	" 定义快捷键保存所有窗口内容并退出vim
	nmap <leader>WQ :wa<CR>:q<CR>
	" 不做任何保存，直接退出vim
	nmap <leader>Q :qa!<CR>
	" 依次遍历子窗口
	nnoremap nw <C-W><C-W>
	" 跳转至右方窗口
	nnoremap <leader>lw <C-W>l
	" 跳转至左方窗口
	nnoremap <leader>hw <C-W>h
	" 跳转至上方的子窗口
	nnoremap <leader>kw <C-W>k
	" 跳转至下方的子窗口
	nnoremap <leader>jw <C-W>j
	" 定义快捷键在结对符之间跳转
	nmap <leader>M %


界面美化
~~~~~~~~~

**主题**

1. ``:echo $VIMRUNTIME`` 查看vim运行目录
#. cd 到运行目录，查看colors文件夹下以.vim结尾的文件
#. 打开~/.vimrc，在其中加入 ``colorscheme 主题名``，保存更改即可

::

	syntax enable
	set background=dark
	colorscheme solarized

*常用命令*

| ``:colorscheme               查看当前主题``
| ``:colorscheme space tab     列出所有主题``
| ``:colorscheme your-theme    切换主题``

**添加辅助信息**

::

	" 总是显示状态栏
	set laststatus=2
	" 显示当前光标位置
	set ruler
	" 开启行号显示
	set number
	" 高亮显示当前行/列
	set cursorline
	set cursorcolumn
	" 高亮显示搜索结果
	set hlsearch
	" 左下角显示当前vim模式
	set showmode

**其他**

::

	" 设置gvim显示字体(提前安装)
	set guifont=YaHei\ Consolas\ Hybrid\ 11.5   #"\"为转义
	" 禁止折行
	set nowrap

代码分析
~~~~~~~~

**语法高亮**

::

	" 开启语法高亮功能
	syntax enable
	" 允许用指定语法高亮配色方案替代默认方案
	syntax on
    
**代码缩进**

::

	" 自适应不同语言的智能缩进
	filetype indent on
	" 将制表符扩展为空格
	set expandtab
	" 设置格式化时制表符占用空格数
	set shiftwidth=4
	" 让vim把连续数量的空格视为一个制表符
	set softtabstop=4
	" 设置编辑时制表符占用空格数
	" set tabstop=4
	" 启用cindent缩进结构
	set cindent

**代码折叠**

::
	
	" 启动vim时关闭代码折叠
	set nofoldenable
	" 基于缩进或语法进行代码折叠
	set foldmethod=indent
	" set foldmethod=syntax

``za        打开或关闭当前折叠``
``zM        关闭所有折叠``
``zR        打开所有折叠``

**代码收藏**

vim的书签分为两类，文件书签和全局书签。

文件书签是标记文件中的不同位置，可以在文件内快速跳转到想要的位置
全局书签是标记不同文件中的位置，可以在不同文件中快速跳转

可以选用可视化书签插件 `vim-signature <https://github.com/kshenoy/vim-signature>`_
打开vim，``:echo has('signs')``，查看是否可以可视化书签

| ``m(a-zA-Z)  创建一个书签a，大写字母是全局书签``
| ```a         跳转到书签a的精确位置``
| ``'a         跳转到书签a所在行的起始位置``
| ``:marks     显示所有书签``
| ``:marks a   显示名称为a书签的详细信息``
| ```.         跳转到最后一次执行改变的精确位置``
| ``'.         跳转到最后一次执行改变的行起始位置``
| ``:delm a    删除标签a``
| ``:delm!     删除全部标签``

**标识符列表**

插件 `Gtags`_

**函数跳转**

插件 `YouCompleteMe`_

**内容查找**

::

	set ic
	set hls
	set is

插件 `ctrlsf`_

**内容替换**

插件 `vim-multiple-cursors`_

**取消备份**

::

	set nobackup
	set noswapfile

**文件编码**

::

	set encoding=utf-8

代码开发
~~~~~~~~~

**快速开关注释**

插件 `NERD Commenter`_

**模板补全**

插件 `UltiSnips`_

**智能补全**

插件 `YouCompleteMe`_

**由接口快速生成实现框架**

**库信息参考**

工程管理
~~~~~~~~~~~~

**工程文件浏览**

插件 `NERDTree`_

**多文档编辑**

vim 的多文档编辑涉及三个概念：buffer、window、tab，这三个事物与我们常规理解意义大相径庭。vim 把加载进内存的文件叫做 buffer，buffer 不一定可见；若要 buffer 要可见，则必须通过 window 作为载体呈现；同个看面上的多个 window 组合成一个 tab。一句话，vim 的 buffer、window、tab 你可以对应理解成视角、布局、工作区。

插件 `MiniBufExplorer`_

**环境恢复**


插件配置
--------
**使用vim-plug 管理插件**

1. 安装vim-plug插件管理器(下载对应的plug.vim到autoload目录即可)

::

	mkdir ~/.vim/autoload/
	cd ~/.vim/autoload/
	wget https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

	
或者使用curl工具来简化下载过程

::

	curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
		https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim


2. vim-plug配置
	
  只需在~/.vimrc增加以下配置段即可(“#”为注释，不需要写)

::

	# 显式指定vim插件存放的位置
	call plug#begin('~/.vim/plugged') 
	# 使用缩写指定了插件在github的地址(https://github.com/junegunn/vim-easy-align)  
	Plug 'junegunn/vim-easy-align'
	# 使用完整的URL指定插件在github的位置
	Plug 'https://github.com/junegunn/vim-github-dashboard.git'
	# 将两个插件写在同一行中
	Plug 'SirVer/ultisnips' | Plug 'honza/vim-snippets'
	# 使用**按需加载**，表明只用在NERTreeToggle命令被调用时，对应插件才会被加载
	Plug 'scrooloose/nerdtree', {'on': 'NERDTreeToggle'}
	# 使用按需加载，表明只有编辑clojure类型的文件时该插件才会被打开
	Plug 'tpope/vim-fireplace', {'for': 'clojure'}
	# 显示指定使用YCM-Generator插件的stable分支
	Plug 'rdnetto/YCM-Generator', {'branch': 'stable'}
	# 指定插件所用的git标签，rtp描述了包含vim插件的子目录
	Plug 'nsf/gocode', {'tag': 'v.20150303', 'rtp': 'vim'}
	# 用dir单独指定该插件存放目录，do用于Post-update hook，指定在安装或更新完插件后所需要执行的额外操作
	Plug 'junegunn/fzf', {'dir': '~/.fzf', 'do': './install --all'}
	# 表示不用github托管本地的vim插件
	Plug '~/my-protptype-plugin'
	# 用于标识vim-plug配置的结束
	call plug#end()

3. 使用vim-plug安装vim插件(需要先在.vimrc中配置才能安装)

  在 ~/.vim 目录下创建一个plugged文件夹，插件装在该目录下

  配置完.vimrc，重启vim，在vim命令行模式下，使用命令 ``:PlugInstall``
  可安装vim配置文件中所有配置的vim插件，也可使用 ``:PlugInstall [name ...]``
  来指定安装某一个或某几个vim插件

4. 查看插件帮助文档

  ``:help NERDTree`` 查看NERDTree插件帮助文档

常用插件
---------

`NERDTree <https://github.com/preservim/nerdtree>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**.vimrc 配置**

::

	[hyun@hyun-ThinkPad-T430s:]$vim ~/.vimrc

	" NERDTree相关设置
	" 启动vim时自动打开NERDTree
	autocmd VimEnter * NERDTree
	" 启动vim时自动打开NERDTree，vim没有文件时，自动退出NERDTree
	" autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") &&b:NERDTreeType == "primary") | q | endif
	" 设置<F5>显示隐藏NERDTree
	map <F5> :NERDTreeMirror<CR>
	map <F5> :NERDTreeToggle<CR>
	" 设置NERDTree放置在编辑区域，1右边，0左边
	let NERDTreeShowHidden=0
	" 设置NERDTree的宽度，默认为30
	let NERDTreeWinPos=40
	" 光标自动显示在编辑区
	autocmd VimEnter * wincmd w
	" 显示隐藏文件
	" let NERDTreeShowHidden=1
	" 当打开NERDTree窗口时，自动显示Bookmarks
	" let NERDTreeShowBookmarks=1
	
**切换工作台**

| ``ctrl+w+h         光标focus左侧树形目录``
| ``ctrl+w+l         光标focus右侧文件显示窗口``
| ``ctrl+w+w         光标自动在窗口之间切换``
| ``ctrl+w+r         移动当前窗口的布局位置``

**切换目录**

| ``o      在已有的窗口中打开文件/目录/书签，并跳到该窗口``
| ``go     在已有的窗口中打开文件/目录/书签，光标保持在树形目录``
| ``t      在新Tab中打开选中文件/书签，并跳到新Tab``
| ``T      在新Tab中打开选中文件/书签，但不跳到新Tab``
| ``i      split 一个新窗口中打开选中文件，并跳到该窗口``
| ``gi     split 一个新窗口中打开选中文件，但不跳到该窗口``
| ``s      vsplit 一个新窗口中打开选中文件，并跳到该窗口``
| ``gs     vsplit 一个新窗口中打开选中文件，但不跳到该窗口``
| ``!      执行当前文件``
| ``o      递归打开选中结点下的所有目录``
| ``O      递归打开选中结点下的所有目录``
| ``x      合拢选中结点的父目录``
| ``X      递归合拢选中结点下的所有目录``
| ``e      edit the current dif``

| ``双击   相当于NERDTree-o``
| ``中键   对文件相当于NERDTree-i,对目录相当于NERDTree-e``

| ``D      删除当前书签``

| ``P      跳到根结点``
| ``p      跳到父结点``
| ``K      跳到当前目录下同级的第一个结点``
| ``J      跳到当前目录下同级的最后一个结点``
| ``k      跳到当前目录下同级的前一个结点``
| ``j      跳到当前目录下同级的后一个结点``

| ``C      将选中目录或选中文件的父目录设为根结点``
| ``u      将当前根结点的父目录设置为根目录，并变成合拢原根结点``
| ``U      将当前根结点的父目录设置为根目录，并保持展开原根结点``
| ``r      递归刷新选中目录``
| ``R      递归刷新根结点``
| ``m      显示文件系统菜单``
| ``cd     将CWD设为选中目录``

| ``I      切换是否显示隐藏文件``
| ``f      切换是否使用文件过滤器``
| ``F      切换是否显示文件``
| ``B      切换是否显示书签``

| ``q      关闭NERDTree窗口``
| ``?      切换是否显示Quick Help``

**切换标签页**

| ``:tabnew [++option选项] [+cmd] 文件    建立对指定文件新的 tab``
| ``:tabc   关闭当前 tab``
| ``:tabo   关闭所有其他 tab``
| ``:tabs   查看所有打开的 tab``
| ``:tabp   前一个 tab``
| ``:tabn   后一个 tab``

标准模式下

| ``gT      前一个 tab``
| ``gt      后一个 tab``


Gtags
~~~~~~

**安装**

\ http://mirror.hust.edu.cn/gnu/global/ 中下载最新的压缩包并解压

| ``cd global-6.6``
| ``./configure``
| ``make``
| ``sudo make install``

`使用vim-plug插件安装Gtags <https://www.gnu.org/software/global/>`_

1. 在代码根目录下执行 ``$ gtags``
#. 打开vim，查询 ``:Gtags func``

`YouCompleteMe <https://github.com/ycm-core/YouCompleteMe>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

增加以下快捷键到.vimrc

::

	nnoremap <leader>jc :YcmCompleter GoToDeclaration<CR>
	" 只能是 #include 或已打开的文件
	nnoremap <leader>jd :YcmCompleter GoToDefinition<CR>

`ctrlsf <https://github.com/dyng/ctrlsf.vim>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**安装**

确保安装了ack

| ``$sudo apt-get update``
| ``$sudo apt-get install ack-grep``
| ``$ack hello # 查找hello``

::

	Plug 'dyng/ctrlsf.vim'

	" 使用 ctrlsf.vim 插件在工程内全局查找光标所在关键字，设置快捷键
	nnoremap <Leader>sp :CtrlSF<CR>

`vim-multiple-cursors <https://github.com/terryma/vim-multiple-cursors>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


`NERD Commenter <https://github.com/preservim/nerdcommenter>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**安装**

::

	Plug 'preservim/nerdcommenter'

``:help nerdcommenter``

**配置**

::

	" 注释后自动加空格
	let g:NERDSpaceDelims=1
	" 使用简洁的语法来修饰多行注释
	let g:NERDCompactSexyComs=1
	" 按行注释分割符左对齐，而不是跟随代码缩进
	let g:NERDDefaultAlign = 'left'
	" 添加自定义格式覆盖默认值
	let g:NERDCustomDelimiters = { 'c': { 'left': '/**','right': '*/' } }
	" 允许注释并反转空行
	let g:NERDTrimTrailingWhitespace = 1
	" 启用NerdCommentToggle以检查所有选定的行是否有注释
	let g:NERDToggleCheckAllLines = 1

默认快捷键(无需配置)

::

	" 注释
	[count]<leader>cc |NERDCommenterComment|
	" 取消注释
	[count]<leader>cu |NERDCommenterUncomment|
	" 切换所选行的注释状态
	[count]<leader>c<space> |NERDCommenterToggle|

`UltiSnips <https://github.com/SirVer/ultisnips>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

`MiniBufExplorer <https://github.com/fholgado/minibufexpl.vim>`_
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

参考文档
--------

| `像IDE一样使用Vim <https://www.bookstack.cn/read/use_vim_as_ide/README.md>`_
| `Vim的终极配置方案，完美的写代码界面! <https://blog.csdn.net/amoscykl/article/details/80616688?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight>`_
| `如何让 vim 成为我们的神器 <https://segmentfault.com/a/1190000011466454>`_
| `vim-plug介绍 <https://vimjc.com/vim-plug.html>`_
| `vim之NERDTree <https://xenojoshua.com/2011/04/vim-nerdtree-chinese-manual/>`_
| `Settings for analyzing code using gnu global in vim <https://slowstarter80.github.io/vim/vim_settings/>`_


