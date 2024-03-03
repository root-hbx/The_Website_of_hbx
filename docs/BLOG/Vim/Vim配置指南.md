### 前言
>[1] Vscode 编辑器之神 <=> Vim 神之编辑器
>[2] 众所周知[Vim](https://github.com/vim/vim)是程序员写代码的“大杀器”
>[3] 虽然Vim是公认的难学，但是一旦你熟练使用，它将使你的写代码效率提升百倍
### 文章定位
- 这是我的Vim学习指南，包含初期“半原生”配置、基本键位语法、最终配置成果
- 本文是一份面向零基础的新手教程，内容极为基础，但是学习顺序因人而异
### 学习指南
1. 基本的命令行和配置文件要相当清楚
2. 先学习一些Vim的基础知识[(看Tutorial即可，时间成本20min)](https://missing.csail.mit.edu/2020/editors/)，再了解有关配置信息[(多看几份“高手”的Vimrc配置文件，从中提炼，时间成本30min)](https://gitlab.com/wsdjeg/vim-galore-zh_cn)
3. 学习相关键位，自定义一套属于自己的“华山剑法”
4. 日常生活中多加练习，在刻意练习Vim的阶段，建议将Vim作为写代码生活的“唯一伴侣”，实在没办法再找Vscode“红杏出墙”hh
5. 建议整理出属于自己的Vim文档，可以拿本文作为一个参考模板，有利于归纳总结与快速查询
6. 共计入门学习时长约10+小时
### 推荐资料
1. 个人认为最适合新手入门的讲解： [一份“接地气”的Vim入门视频](https://www.bilibili.com/video/BV164411P7tw/?spm_id_from=333.337.search-card.all.click&vd_source=8a3dd36862125e80dc439254ef65d959)
2. 新手入门后可以小试牛刀，看看进阶：[“接地气”后记](https://www.bilibili.com/video/av55664166/?vd_source=8a3dd36862125e80dc439254ef65d959)
3. 个人认为最系统的讲解：[A-Missing-Semester_Vim_Video](https://www.youtube.com/watch?v=a6Q8Na575qc&t=2s)
4. 一份深入了解Vim配置信息的资源（大佬专用orz）：[Learn_Vim_Script_the_Hard_Way](https://learnvimscriptthehardway.stevelosh.com/)
### 本文配置说明
- 配置设备：MacbookPro Sonoma14.0
- 键盘映射：美式键盘
- vim位置：/opt/homebrew/bin/vim
- python3位置：/opt/homebrew/bin/python3
### 我的半原生Vim配置
在主文件夹下：vim .vimrc

下面这份配置清单展示的是另一台在电脑上的配置文件
>Linux root-hbx 6.5.0-17-generic #17~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Jan 16 14:32:32 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
### root-hbx                    
    description: Notebook
    product: 21CY (LENOVO_MT_21CY_BU_idea_FM_ThinkBook 16 G4+ IAP)
    vendor: LENOVO
    version: ThinkBook 16 G4+ IAP
    serial: YX04FKHS
    width: 64 bits
    capabilities: smbios-3.3.0 dmi-3.3.0 smp vsyscall32
```vim
set nocompatible
filetype on
 
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
 
 
" 这里根据自己需要的插件来设置，以下是我的配置 "
"
" YouCompleteMe:语句补全插件
set runtimepath+=~/.vim/bundle/YouCompleteMe
autocmd InsertLeave * if pumvisible() == 0|pclose|endif "离开插入模式后自动关闭预览窗口"
let g:ycm_collect_identifiers_from_tags_files = 1           " 开启 YCM基于标签引擎
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释与字符串中的内容也用于补全
let g:syntastic_ignore_files=[".*\.py$"]
let g:ycm_seed_identifiers_with_syntax = 1                  " 语法关键字补全
let g:ycm_complete_in_comments = 1
let g:ycm_confirm_extra_conf = 0                            " 关闭加载.ycm_extra_conf.py提示
let g:ycm_key_list_select_completion = ['<c-n>', '<Down>']  " 映射按键,没有这个会拦截掉tab, 导致其他插件的tab不能用.
let g:ycm_key_list_previous_completion = ['<c-p>', '<Up>']
let g:ycm_complete_in_comments = 1                          " 在注释输入中也能补全
let g:ycm_complete_in_strings = 1                           " 在字符串输入中也能补全
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全
let g:ycm_global_ycm_extra_conf='~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp/ycm/.ycm_extra_conf.py'
let g:ycm_show_diagnostics_ui = 0                           " 禁用语法检查
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<CR>"             " 回车即选中当前项
nnoremap <c-j> :YcmCompleter GoToDefinitionElseDeclaration<CR>     " 跳转到定义处
let g:ycm_min_num_of_chars_for_completion=2                 " 从第2个键入字符就开始罗列匹配项
"
 
 
 
" github 仓库中的插件 "
Plugin 'VundleVim/Vundle.vim'
 
 
Plugin 'vim-airline/vim-airline'
"vim-airline配置:优化vim界面"
"let g:airline#extensions#tabline#enabled = 1
" airline设置
" 显示颜色
set t_Co=256
set laststatus=2
" 使用powerline打过补丁的字体
let g:airline_powerline_fonts = 1
" 开启tabline
let g:airline#extensions#tabline#enabled = 1
" tabline中当前buffer两端的分隔字符
let g:airline#extensions#tabline#left_sep = ' '
" tabline中未激活buffer两端的分隔字符
let g:airline#extensions#tabline#left_alt_sep = ' '
" tabline中buffer显示编号
let g:airline#extensions#tabline#buffer_nr_show = 1
" 映射切换buffer的键位
nnoremap [b :bp<CR>
nnoremap ]b :bn<CR>
" 映射<leader>num到num buffer
map <leader>1 :b 1<CR>
map <leader>2 :b 2<CR>
map <leader>3 :b 3<CR>
map <leader>4 :b 4<CR>
map <leader>5 :b 5<CR>
map <leader>6 :b 6<CR>
map <leader>7 :b 7<CR>
map <leader>8 :b 8<CR>
map <leader>9 :b 9<CR>
 
 
 
" vim-scripts 中的插件 "
Plugin 'taglist.vim'
"ctags 配置:F3快捷键显示程序中的各种tags，包括变量和函数等。
map <F3> :TlistToggle<CR>
let Tlist_Use_Right_Window=1
let Tlist_Show_One_File=1
let Tlist_Exit_OnlyWindow=1
let Tlist_WinWidt=25
 
Plugin 'The-NERD-tree'
"NERDTree 配置:F2快捷键显示当前目录树
map <F2> :NERDTreeToggle<CR>
let NERDTreeWinSize=25 
 
Plugin 'indentLine.vim'
Plugin 'delimitMate.vim'
 
" 非 github 仓库的插件"
" Plugin 'git://git.wincent.com/command-t.git'
" 本地仓库的插件 "
 
call vundle#end()
 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""新文件标题
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"新建.c,.h,.sh,.java文件，自动插入文件头 
autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()" 
""定义函数SetTitle，自动插入文件头 
func SetTitle() 
	"如果文件类型为.sh文件 
	if &filetype == 'sh' 
		call setline(1, "##########################################################################") 
		call append(line("."), "# File Name: ".expand("%")) 
		call append(line(".")+1, "# Author: amoscykl") 
		call append(line(".")+2, "# mail: amoscykl980629@163.com") 
		call append(line(".")+3, "# Created Time: ".strftime("%c")) 
		call append(line(".")+4, "#########################################################################") 
		call append(line(".")+5, "#!/bin/zsh")
		call append(line(".")+6, "PATH=/home/edison/bin:/home/edison/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/work/tools/gcc-3.4.5-glibc-2.3.6/bin")
		call append(line(".")+7, "export PATH")
		call append(line(".")+8, "")
	else 
		call setline(1, "/*************************************************************************") 
		call append(line("."), "	> File Name: ".expand("%")) 
		call append(line(".")+1, "	> Author: amoscykl") 
		call append(line(".")+2, "	> Mail: amoscykl@163.com ") 
		call append(line(".")+3, "	> Created Time: ".strftime("%c")) 
		call append(line(".")+4, " ************************************************************************/") 
		call append(line(".")+5, "")
	endif
	if &filetype == 'cpp'
		call append(line(".")+6, "#include<iostream>")
    	call append(line(".")+7, "using namespace std;")
		call append(line(".")+8, "")
	endif
	if &filetype == 'c'
		call append(line(".")+6, "#include<stdio.h>")
		call append(line(".")+7, "")
	endif
	"	if &filetype == 'java'
	"		call append(line(".")+6,"public class ".expand("%"))
	"		call append(line(".")+7,"")
	"	endif
	"新建文件后，自动定位到文件末尾
	autocmd BufNewFile * normal G
endfunc 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"键盘命令
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
 
nmap <leader>w :w!<cr>
nmap <leader>f :find<cr>
 
" 映射全选+复制 ctrl+a
map <C-A> ggVGY
map! <C-A> <Esc>ggVGY
map <F12> gg=G
" 选中状态下 Ctrl+c 复制
vmap <C-c> "+y
 
 
 
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
""实用设置
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" 设置当文件被改动时自动载入
set autoread
" quickfix模式
autocmd FileType c,cpp map <buffer> <leader><space> :w<cr>:make<cr>
"代码补全 
set completeopt=preview,menu 
"允许插件  
filetype plugin on
"共享剪贴板  
set clipboard=unnamed 
"从不备份  
set nobackup
"make 运行
:set makeprg=g++\ -Wall\ \ %
"自动保存
set autowrite
set ruler                   " 打开状态栏标尺
set cursorline              " 突出显示当前行
set magic                   " 设置魔术
set guioptions-=T           " 隐藏工具栏
set guioptions-=m           " 隐藏菜单栏
"set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\
" 设置在状态行显示的信息
set foldcolumn=0
set foldmethod=indent 
set foldlevel=3 
set foldenable              " 开始折叠
" 不要使用vi的键盘模式，而是vim自己的
set nocompatible
" 语法高亮
set syntax=on
" 去掉输入错误的提示声音
set noeb
" 在处理未保存或只读文件的时候，弹出确认
set confirm
" 自动缩进
set autoindent
set cindent
" Tab键的宽度
set tabstop=4
" 统一缩进为4
set softtabstop=4
set shiftwidth=4
" 不要用空格代替制表符
set noexpandtab
" 在行和段开始处使用制表符
set smarttab
" 显示行号
set number
" 历史记录数
set history=1000
"禁止生成临时文件
set nobackup
set noswapfile
"搜索忽略大小写
set ignorecase
"搜索逐字符高亮
set hlsearch
set incsearch
"行内替换
set gdefault
"编码设置
set enc=utf-8
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
"语言设置
set langmenu=zh_CN.UTF-8
set helplang=cn
" 我的状态行显示的内容（包括文件类型和解码）
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}
set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c][%p%%]
" 总是显示状态行
set laststatus=2
" 命令行（在状态行下）的高度，默认为1，这里是2
set cmdheight=2
" 侦测文件类型
filetype on
" 载入文件类型插件
filetype plugin on
" 为特定文件类型载入相关缩进文件
filetype indent on
" 保存全局变量
set viminfo+=!
" 带有如下符号的单词不要被换行分割
set iskeyword+=_,$,@,%,#,-
" 字符间插入的像素行数目
set linespace=0
" 增强模式中的命令行自动完成操作
set wildmenu
" 使回格键（backspace）正常处理indent, eol, start等
set backspace=2
" 允许backspace和光标键跨越行边界
set whichwrap+=<,>,h,l
" 可以在buffer的任何地方使用鼠标（类似office中在工作区双击鼠标定位）
set mouse=a
set selection=exclusive
set selectmode=mouse,key
" 通过使用: commands命令，告诉我们文件的哪一行被改变过
set report=0
" 在被分割的窗口间显示空白，便于阅读
set fillchars=vert:\ ,stl:\ ,stlnc:\
" 高亮显示匹配的括号
set showmatch
" 匹配括号高亮的时间（单位是十分之一秒）
set matchtime=1
" 光标移动到buffer的顶部和底部时保持3行距离
set scrolloff=3
" 为C程序提供自动缩进
set smartindent
" 高亮显示普通txt文件（需要txt.vim脚本）
 au BufRead,BufNewFile *  setfiletype txt
"自动补全
:inoremap ( ()<ESC>i
:inoremap ) <c-r>=ClosePair(')')<CR>
":inoremap { {<CR>}<ESC>O
":inoremap } <c-r>=ClosePair('}')<CR>
:inoremap [ []<ESC>i
:inoremap ] <c-r>=ClosePair(']')<CR>
:inoremap " ""<ESC>i
:inoremap ' ''<ESC>i
function! ClosePair(char)
	if getline('.')[col('.') - 1] == a:char
		return "\<Right>"
	else
		return a:char
	endif
endfunction
filetype plugin indent on 
"打开文件类型检测, 加了这句才可以用智能补全
set completeopt=longest,menu
```
### Vim入门讲解
- Vim操作命令的本质：<\option>[命令]<\motion>
- 我的日常工作流中，主要使用五种模式：**命令模式（Command Mode）、输入模式（Insert Mode）、可视化模式（Visual Mode）和命令行模式（Command-Line Mode）、搜索模式(Search Mode)**
#### 1. 命令模式 [Command Mode]
- `i` -- 切换到输入模式，在光标当前位置开始输入文本
- `x` -- 删除当前光标所在处的字符
- `:` -- 切换到 *底线命令模式*，以在最底一行输入命令
- `\` -- 切换到 *底线搜索模式*，以在最底一行输入命令
- `a` -- 进入插入模式，在光标下一个位置开始输入文本
- `o` -- 在当前行的下方插入一个新行，并进入插入模式
- `O` -- 在当前行的上方插入一个新行，并进入插入模式
- `dd` -- 删除当前行
- `yy` -- 复制当前行
- `p` -- 粘贴剪贴板内容到光标下方
- `P` -- 粘贴剪贴板内容到光标上方
- `u` -- 撤销上一次操作
- `Ctrl + r` -- 重做上一次撤销的操作
- `:w` -- 保存文件
- `:q` -- 退出 Vim 编辑器
- `:q!` -- 强制退出Vim 编辑器，不保存修改
##### 我认为在Command_Mode比较“偷懒”的操作:
**快速跳转到行首和行末**
>0：跳转到行首
$：跳转到行尾

**快速跳转到文首和文末**
>gg：跳转到文档首
shift+g(<=>G)：跳转到文档尾

**d: 删除**
>d→: 向右边delete一个字符
d←: 向左边delete一个字符
d3→：向右边delete 3个字符
d3←：向左边delete 3个字符

**y: 复制** 
>y→: 向右边yank一个字符
y←: 向左边yank一个字符
y3→：向右边yank 3个字符
y3←：向左边yank 3个字符

**??：对这一行执行某一操作**
>dd: 删除[<=>剪切]这一行(delete) 
yy: 复制这一行(yank)
p: 粘贴这一行(paste)

**三个单词锁定的操作**
>- w: 直接跳转到同一行内的*下一个单词首字母* / 直至当前光标所在单词*末尾*
>- b: 直接跳转回同一行内的*上一个单词首字母* / 回溯至当前光标所在单词*开头*
>- i:  针对*词中*

**c: 改变(<=>d + into->WritingMode)**
>c3→：向右边delete 3个字符，随后进入Writing模式
cw: 删除当前光标到该*单词末*之间的全部字符

>场景1：(eg: The op wi|ndows is ... ==欲删除单词windows并瞬间改写它==)
>- ciw: 光标在单词中间时改变单词
>- bcw: 光标回退到*单词开头*，再执行cw操作(上文)

>场景2：(eg: "The op wi|ndows" ==欲改写” “内所有的内容==)
>- ci" : 删除" "内所有内容并进行改写

>场景3：(eg: <The op wi|ndows> ==欲改写<>内所有的内容==)
>- ci< : 删除< >内所有内容并进行改写
>其余类比推理！

**f：寻找**
f?: 寻找从当前光标往后，第一个是“字符?”的位置
yf?: *从当前光标的位置* 一直复制到 *"字符?"所在的位置*
df?: *从当前光标的位置* 一直删除到 *"字符?"所在的位置*
cf?: *从当前光标的位置* 一直删除到 *"字符?"所在的位置*，并进入Writing_Mode
#### 2. 输入模式 [Insert Mode]
在命令模式下按下 i 就进入了输入模式，使用 `Esc 键`可以返回到普通模式。
跟正常的输入一模一样，略之！
#### 3. 可视化模式 [Visual Mode]
v : *普通可视模式*，选中文本（见Appendix）
shift + v (<=> V): *行可视模式*，选中行文本（见Appendix）

- 按照上述两种模式后，可以进行前述的基本操作
>如: c(改变) / y(复制)......
- 还可以进行一些“整体性”的操作：
>指令：进入v/V模式后，输入：`:normal 指令+内容`
>比如下面展示的就是：在row13-18内容行首加上“hello-hbx-0-”前缀
>![|350](Zpng/Pasted%20image%2020240213181231.png)
效果：
![|350](Zpng/Pasted%20image%2020240213181256.png)


#### 4. 底线命令模式 \\ 命令行模式 [Command-Line Mode]
- `:w`：保存文件
- `:q`：退出 Vim 编辑器
- `:wq`：保存文件并退出 Vim 编辑器
- `:q!`：强制退出Vim编辑器，不保存修改
#### 5. 底线搜索模式  [Search-Line Mode]
- `/[搜索内容]`：在全文内进行搜索[指定内容]
### 最终配置清单
```vim
"Clearly this is my configuration of Vim, and it's suitable for macOS!

" 0. 显示设置（优化）
set nocompatible
filetype on
filetype indent on
filetype plugin on
filetype plugin indent on
"let g:ycm_python_binary_path = '/opt/homebrew/bin/python3'
set mouse=a                                 "可使用鼠标控制
set encoding=utf-8
let &t_ut=''
set expandtab                               "缩进相关
set tabstop=4
set shiftwidth=4
set softtabstop=4
"set list                                   "行末空格会用$代替表示，不推荐
"set listchars=tab:...?
set scrolloff=5
set tw=0
set indentexpr=
set backspace=indent,eol,start              "row(k+1)行首 回退至 row(k)行末
set foldmethod=indent
set foldlevel=99
let &t_SI = "\<Esc>]50;CursorShape=1\x7"
let &t_SR = "\<Esc>]50;CursorShape=2\x7"
let &t_EI = "\<Esc>]50;CursorShape=0\x7"    "三种模式下光标显示不一样
set laststatus=2                            "最下面的状态栏宽度是2
set autochdir
au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" |endif


let mapleader = " "             "将键盘<LEADER>主动设置为空格键<SPACE>
syntax on                       "语法高亮
set number                      "设置行号
set norelativenumber            "设置相对行号（off）
set cursorline                  "行所在下线
set wrap
set showcmd
set wildmenu                    "命令关键字自索引
set nohlsearch                  "搜索高亮
exec "nohlsearch"
set incsearch                   "搜索过程直接高亮
set ignorecase                  "大小写忽略(searching)
set smartcase                   "大小写自动识别(searching)


" 1. 将hjkl重定向给四个方向键
noremap <Up> k
noremap <Down> j
noremap <Left> h
noremap <Right> l


" 2. 常见键位映射更改
" 1) shift + s <=> (S) => 实现保存 <=> :w<CR> (<CR>是回车键)
" 2) s 原：删除所在字符并自开启insert模式 => none
" 3) shift + q <=> (Q) => 实现退出 <=> :q<CR>
" 4) shift + r <=> (R) => 刷新.vimrc_File <=> :source $MYVIMRC<CR>
" map S :w<CR>
map s <nop>
map Q :wq<CR>
map R :source $MYVIMRC<CR>

" 3. search jumping
noremap = nzz                         "= refers to 跳到下一个搜索目标
noremap - Nzz                         "- refers to 返回上一个搜索目标
noremap <LEADER><CR> :nohlsearch<CR>  "<SPACE>+<return> refers to 撤销搜索印记


" 4. 实现高级分屏和跳转
" 高级分屏
map s<Right> :set splitright<CR>:vsplit<CR>
map s<Left> :set nosplitright<CR>:vsplit<CR>
map s<Up> :set nosplitbelow<CR>:split<CR>
map s<Down> :set splitbelow<CR>:split<CR>
" 鼠标跳转
map q<Right> <C-w>l
map q<Left>  <C-w>h
map q<Up>    <C-w>k
map q<Down>  <C-w>j
" 设置当前分屏大小
map w<Right> :vertical resize-5<CR>
map w<Left>  :vertical resize+5<CR>
map w<Up> :res -5<CR>
map w<Down> :res +5<CR>
" 分屏在“上下”&“左右”之间切换
map sv <C-w>t<C-w>H
map sh <C-w>t<C-w>K


"5. 设置打开一个新的标签页
map tu :tabe<CR>
map ty :-tabnext<CR>
map ti :+tabnext<CR>

"-----------------------------------------------------------------------

" end: set plugins
call plug#begin('~/.vim/plugged')

Plug 'vim-airline/vim-airline'
Plug 'connorholyday/vim-snazzy'

" File navigation
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
Plug 'Xuyuanp/nerdtree-git-plugin'

" Taglist
Plug 'majutsushi/tagbar', { 'on': 'TagbarOpenAutoClose' }

" Error checking
Plug 'w0rp/ale'

" Auto Complete
Plug 'Valloric/YouCompleteMe'

" Undo Tree
Plug 'mbbill/undotree/'

" Other visual enhancement
Plug 'nathanaelkane/vim-indent-guides'
Plug 'itchyny/vim-cursorword'

" Git
Plug 'rhysd/conflict-marker.vim'
Plug 'tpope/vim-fugitive'
Plug 'mhinz/vim-signify'
Plug 'gisphm/vim-gitignore', { 'for': ['gitignore', 'vim-plug'] }

" HTML, CSS, JavaScript, PHP, JSON, etc.
Plug 'elzr/vim-json'
Plug 'hail2u/vim-css3-syntax'
Plug 'spf13/PIV', { 'for' :['php', 'vim-plug'] }
Plug 'gko/vim-coloresque', { 'for': ['vim-plug', 'php', 'html', 'javascript', 'css', 'less'] }
Plug 'pangloss/vim-javascript', { 'for' :['javascript', 'vim-plug'] }
Plug 'mattn/emmet-vim'

" Python
Plug 'vim-scripts/indentpython.vim'

" Markdown
Plug 'iamcco/markdown-preview.nvim', { 'do': { -> mkdp#util#install_sync() }, 'for' :['markdown', 'vim-plug'] }
Plug 'dhruvasagar/vim-table-mode', { 'on': 'TableModeToggle' }
Plug 'vimwiki/vimwiki'

" Bookmarks
Plug 'kshenoy/vim-signature'

" Other useful utilities
Plug 'terryma/vim-multiple-cursors'
Plug 'junegunn/goyo.vim' " distraction free writing mode
Plug 'tpope/vim-surround' " type ysks' to wrap the word with '' or type cs'` to change 'word' to `word`
Plug 'godlygeek/tabular' " type ;Tabularize /= to align the =
Plug 'gcmt/wildfire.vim' " in Visual mode, type i' to select all text in '', or type i) i] i} ip
Plug 'scrooloose/nerdcommenter' " in <space>cc to comment a line

" Dependencies
Plug 'MarcWeber/vim-addon-mw-utils'
Plug 'kana/vim-textobj-user'
Plug 'fadein/vim-FIGlet'

call plug#end()

"-----------------------------------------------------------------------

" end+: colorBar
color snazzy
let g:SnazzyTransparent = 1

" ===
" === NERDTree
" ===
map ff :NERDTreeToggle<CR>
let NERDTreeMapOpenExpl = ""
let NERDTreeMapUpdir = ""
let NERDTreeMapUpdirKeepOpen = "l"
let NERDTreeMapOpenSplit = ""
let NERDTreeOpenVSplit = ""
let NERDTreeMapActivateNode = "i"
let NERDTreeMapOpenInTab = "o"
let NERDTreeMapPreview = ""
let NERDTreeMapCloseDir = "n"
let NERDTreeMapChangeRoot = "y"


" ==
" == NERDTree-git
" ==
let g:NERDTreeIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ "Unknown"   : "?"
    \ }


" ===
" === You Complete ME
" ===
nnoremap gd :YcmCompleter GoToDefinitionElseDeclaration<CR>
nnoremap g/ :YcmCompleter GetDoc<CR>
nnoremap gt :YcmCompleter GetType<CR>
nnoremap gr :YcmCompleter GoToReferences<CR>
let g:ycm_autoclose_preview_window_after_completion=0
let g:ycm_autoclose_preview_window_after_insertion=1
let g:ycm_use_clangd = 0
let g:ycm_python_interpreter_path = "/opt/homebrew/bin/python3"
let g:ycm_python_binary_path = "/opt/homebrew/bin/python3"


" ===
" === ale
" ===
let b:ale_linters = ['pylint']
let b:ale_fixers = ['autopep8', 'yapf']


" ===
" === Taglist
" ===
map <silent> T :TagbarOpenAutoClose<CR>


" ===
" === MarkdownPreview
" ===
let g:mkdp_auto_start = 0
let g:mkdp_auto_close = 1
let g:mkdp_refresh_slow = 0
let g:mkdp_command_for_global = 0
let g:mkdp_open_to_the_world = 0
let g:mkdp_open_ip = ''
let g:mkdp_browser = 'chromium'
let g:mkdp_echo_preview_url = 0
let g:mkdp_browserfunc = ''
let g:mkdp_preview_options = {
    \ 'mkit': {},
    \ 'katex': {},
    \ 'uml': {},
    \ 'maid': {},
    \ 'disable_sync_scroll': 0,
    \ 'sync_scroll_type': 'middle',
    \ 'hide_yaml_meta': 1
    \ }
let g:mkdp_markdown_css = ''
let g:mkdp_highlight_css = ''
let g:mkdp_port = ''
let g:mkdp_page_title = '「${name}」'


" ===
" === vim-table-mode
" ===
map <LEADER>tm :TableModeToggle<CR>

" ===
" === Python-syntax
" ===
let g:python_highlight_all = 1
" let g:python_slow_sync = 0


" ===
" === vim-indent-guide
" ===
let g:indent_guides_guide_size = 1
let g:indent_guides_start_level = 2
let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_color_change_percent = 1
silent! unmap <LEADER>ig
autocmd WinEnter * silent! unmap <LEADER>ig


" ===
" === vim-signiture
" ===
let g:SignatureMap = {
        \ 'Leader'             :  "m",
        \ 'PlaceNextMark'      :  "m,",
        \ 'ToggleMarkAtline'   :  "m.",
        \ 'PurgeMarksAtLine'   :  "dm-",
        \ 'DeleteMark'         :  "dm",
        \ 'PurgeMarks'         :  "dm/",
        \ 'PurgeMarkers'       :  "dm?",
        \ 'GotoNextLineAlpha'  :  "m<LEADER>",
        \ 'GotoPrevLineAlpha'  :  "",
        \ 'GotoNextSpotAlpha'  :  "m<LEADER>",
        \ 'GotoPrevSpotAlpha'  :  "",
        \ 'GotoNextLineByPos'  :  "",
        \ 'GotoPrevLineByPos'  :  "",
        \ 'GotoNextSpotByPos'  :  "mn",
        \ 'GotoPrevSpotByPos'  :  "mp",
        \ 'GotoNextMarker'     :  "",
        \ 'GotoPrevMarker'     :  "",
        \ 'GotoNextMarkerAny'  :  "",
        \ 'GotoPrevMarkerAny'  :  "",
        \ 'ListLocalMarks'     :  "m/",
        \ 'ListLocalMarkers'   :  "m?"
        \ }


" ===
" === Undotree
" ===
let g:undotree_DiffAutoOpen = 0
map L :UndotreeToggle<CR>


huluobo@huluobodeMacBook-Pro  ~   main ± 
```
#### 配置效果：


#### Tips:
##### 1. 在Vim中，常见的Mac键盘按键在字母表示法中可以使用以下方式表示：
1. **Control键（<\C>）**:
    - 在Vim中，使用`<C-[键]`来表示。例如，`<C-c>` 表示按下Control键和'C'键
2. **Option键（<\M>）**:
    - 在Vim中，通常使用`<M-[键]`来表示。例如，`<M-a>` 表示按下Option/Alt键和'a'键
3. **Shift键（<\S>）**:
    - 在Vim中，使用`<S-[键]`来表示。例如，`<S-a>` 表示按下Shift键和'a'键
4. **Command键（<\D>）**:
    - 在Mac中，Command键有时也被称为Super键。在Vim中，使用`<D-[键]`来表示。例如，`<D-a>` 表示按下Command键和'a'键
5. **Function键（<\Fn>）**:
    - Vim通常不会直接捕捉功能键，因此在Vim中，你可能需要使用其他键位的组合来模拟功能键的操作
6. **四个方向键**：
	- `<\Up> <\Down> <\Left> <\Right>`
##### 2. 配置插件的方式：
>[1] 将这一堆插件放进`call plug#begin('~/.vim/plugged')`与`call plug#end()`之间
>[2] 部分插件需要激活，比如YouComplateMe
>>ASK: 如何激活YouComplateMe? 
>>1. cd ~/.vim/plugged              " 进入所有插件的安装文库
>>2. cd YouComplateMe           " 进入YouComplateMe库
>>3. python3 install.py              " 安装install.py
>>4. cd ~                                    " 回到主文件夹
>>5. vim .vimrc
>>6. 将路径改为python3本地路径：`let g:ycm_python_interpreter_path = "/opt/homebrew/bin/python3"` and `let g:ycm_python_binary_path = "/opt/homebrew/bin/python3"`
>[3] 激活成功，可以顺利使用了！
### Appendix
![|500](Zpng/Pasted%20image%2020240213181702.png)![|500](Zpng/Pasted%20image%2020240213180351.png)![|500](Zpng/Pasted%20image%2020240213180251.png)