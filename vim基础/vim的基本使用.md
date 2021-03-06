安装好vim之后，就可以开始使用vim，但是这个时候的vim只有最基本的功能。

# vim的键盘图
vim有非常多的快捷键，下面是vim的基本的键盘图：
![](../picture/vim键盘图.png)

说明：</br>
- 1.$\cdot$ 表示单个字母不是完整的命令，还需要进一步的输入。例如，单个的`g`没有意义，`gg`则表示跳转到开头。</br>
- 2.一个键最多有3排内容，最底下的是直接按键的结果；中间的是按下`shift`的结果(在编辑模式，按下`shift`键的同时输入字符，就是大写，所以其实用不到`caps lock`这个键)；上面偏右是按下`ctrl`的结果。</br>

一些常见的命令：  
- `:!`  执行外部shell命令
- `:r`  读文件,可以读取另外一个文件的内容到本文件中
- `:w`  写文件
- `:e`  打开编辑文件
- `:s`  替换命令
- `:help`  查看帮助文档
- `:set ft?`  插卡当前文件类型


# vim的四种模式
+ 正常模式（normal）,最常见的，按下`esc`就可以回到正常模式；
+ 插入模式（insert），最常见的，按下`i`或`a`就可以进入插入模式，开始编辑文件；
+ 可视模式（visual），最常见的，按下`v`就可以进入可视模式，可以用来选定文本块，包括按行和按列块进行选定（`ctrl-v`）；
+ 命令行模式（command-line），最常见的，按下`:`就可以进入命令行模式，用来执行一些复杂、冗长的命令；注意，使用`/`进行搜索或使用`?`进行搜索也是命令行模式；

vim中的一些特殊键描述（在重映射中也会用到）：
- `<Esc>`表示`esc`键
- `<CR>`表示回车键
- `<Space>`表示空格键
- `<Tab>`表示`Tab`键
- `<BS>` 退格键
- `<Del>` 删除键
- `<lt>`  <键
- `<Up>`  光标上移键
- `<Down>` 光标下移键
- `<Left>`  左移
- `<Right>` 右移
- `<PageUp>` Page Up键
- `<PageDown>` Page Down键
- `<Home>` Home键盘
- `<End>` End键盘
- `<F1>-<F12>`  F1-F12键
- `<S-...>`  表示shift 组合键
- `<C-...>`  表示ctrl 组合键
- `<M-...>`  表示alt 组合键
- `<D-...>`  表示Command 组合键（MAC系统）

# vim的配置文件
vim的配置文件默认存放在~/.vimrc，如果你的系统没有，那么需要自己创建`touch ~/.vimrc`。
