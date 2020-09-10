# .vimrc初步配置
最基本的vim配置可以是下面这个样子：
```
set enc=utf-8
set nocompatible
source $VIMRUNTIME/vimrc_example.vim
```
上面的含义：
- 设置文件的编码为utf-8
- 设置不和vi兼容
- 导入vim的实例配置（可能会打开一些有用的选项，如语法加亮、搜索加亮、命令历史、记住上次文件位置等）

# 备份和撤销文件
对于下面2个设置
```
set backup
set undofile
```
第一个选项让我们的每次编辑都会保留上一次的备份文件，第二个选项让vim在重新打开一个文件的时候，仍然可以撤销(undofile)之前的编辑，因为这个选项额外还产生了一个保留编辑历史的撤销文件，所以会产生额外的两个文件。但是，其实只保留一个撤销文件就够了。所
以，可以像下面这样设置比较合适：
```
set nobackup
set undodir=~/.vim/undodir

if !isdirectory(&undodir)
    call mkdir(&undodir,'p',0700)
endif
```
其中，`set undodir=~/.vim/undodir`将撤销文件放在了用户个人的特定目录下，比较安全独立。最后的一部分设置是为了让vim在启动的时候自动判断撤销文件存放目录是否存在，否则直接创建。可以通过下面的命令查询用到的函数的含义：
```
:help isdirectory()
:help mkdir()
:help :call
```

# 鼠标支持
通常，在vim中使用鼠标的时候，鼠标点击之后，vim会自动进入可视模式，会开始选择文件内容。但是，其实如果你使用 xterm兼容终端的话，可以这样来操作：
- 按下`shift`后使用鼠标，就不会进入可视模式，产生操作系统的文本选择（同时进行复制的话，内容可以给其他程序使用）；
- 不按下`shift`使用鼠标，则进入可视模式，选择文本；

更一般的方法是（不支持`shift`或不兼容xterm），可以采用绕过的方式：让vim在某种情况下不接管鼠标事件（一般是在命令行模式下），可以下面这样配置：
```
if has('mouse')
  if has('gui_running') || (&term =~ 'xterm' && !has('mac'))
    set mouse=a
  else
    set mouse=nvi
  endif
endif
```
含义是vim如果支持鼠标的话，且满足下面任一条件：
- 图形界面正在运行
- 终端兼容xterm，且不是Mac</br>

则启用完全的鼠标支持(`set mouse=a`)，这时候鼠标在vim拖动就会进入可视模式选择文本，但是当用户按下`shift`时，窗口系统接管鼠标，用户可以使用鼠标复制vim中的内容供其他程序使用。

否则就只能在n(正常模式)、v（可视模式）、i（插入模式）中使用鼠标；而在命令行模式的时候(按下`:`)，vim将不对鼠标进行相应，用户可以使用鼠标复制vim中的内容供其他程序使用。





