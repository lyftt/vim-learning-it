# vim 的目录结构
vim的运行支持文件放置的位置跟`make install`时的安装目录有关，但是一般来说可能是下面的这些位置：
- /usr/share/vim/vim82  (vim82是安装的版本)
- /usr/local/share/vim/vim82

这个目录下的配置文件一般有这些：
```
- colors         vim的配色方案
- syntax         vim的语法加亮文件
- doc            vim的帮助文件
- plugin         vim的插件，用来增强vim
- ftplugin       vim针对特定类型文件的插件
- autoload       vim自动载入的脚本
- indent         定义自动缩进的文件
- compiler       vim的编译命令:compiler使用的脚本文件
- tools          工具
- tutor          入门教程
- macros         宏示例
```
其中，使用`:help`命令查看的帮助的文件内容就放在doc目录下。plugin中放的主要是一些系统的内置插件。主要有：
```
getscriptPlugin.vim    获取最新vim插件的脚本（过时了）
logiPat.vim            模式匹配的逻辑运算符 （允许以逻辑运算、而非标准正则表达式的方式来写模式匹配表达式）
matchparen.vim         对括号进行高亮匹配
spellfile.vim          在拼写文件缺失时自动下载
zipPlugin.vim          直接编辑zip文件（和tar文件不同，zip文件可支持写入）
gzip.vim               直接编辑.gz压缩文件
tarPlugin.vim          编辑（压缩的）tar文件（注意，和 gzip 情况不同，这儿不支持写入）
netrwPlugin.vim        从网络编辑浏览文件和目录（支持常见的ftp、scp等协议，可以直接打开目录来选择文件）
manpager.vim           用vim来查看man帮助
等
```
上面的这些插件都可以通过`:help`命令来查看帮助，查看帮助时，插件名称中的`Plugin`后缀需要去掉：查看zip文件编辑的帮助时，应当使用`:help zip`而不是`:help zipPlugin`。

vim的程序运行的许多重要的配置文件都在这个目录下，但是更一般的，用户一般都是“克隆”这个目录。即用户可以“克隆”这个目录结构，一般用户克隆的目录在~/.vim中，这个目录一般由用户用来定制自己的vim。

# vim8 的新功能
- 软件包的支持（`:help packages`）
- 异步任务的支持（`:help channel`、`:help job`、`:help timers`）
- 终端支持（`:help terminal`）

