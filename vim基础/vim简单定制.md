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

vim的程序运行的许多重要的配置文件都在这个目录（安装目录）下，但是更一般的，用户一般都是“克隆”这个目录。即用户可以“克隆”这个目录结构，一般用户克隆的目录在~/.vim中，这个目录一般由用户用来定制自己的vim。

Vim 的安装目录你是不应该去修改的。首先，你可能没有权限去修改这个目录；其次，即使你有修改权限，这个目录会在 Vim 升级时被覆盖，你做的修改也会丢失。用户自己的配置应当放在自己的目录下，这也就是用户自己的主目录下的 Vim 配置目录（Linux下的 .vim，Windows下的vimfiles）。这个目录应和 Vim安装目录下的运行支持文件目录有相同的结构，但下面的子目录你在需要修改 Vim 的相关行为时才有必要创建。如果一个同名文件出现用户自己的 Vim 配置目录里和 Vim 的安装目录里，`用户的文件优先`。

换句话说，修改 Vim 行为最简单的一种方式，就是把一个系统的运行支持文件复制到自己的 Vim 配置目录下的相同位置，然后修改其内容。常常可以用这种方式来精调 Vim 的语法加亮。

关于 Vim 的公用脚本，这儿再多说几句。Vim 的网站过去是用来集中获取各种脚本——如插件和配色方案——的地方，而getscriptPlugin 可以帮助简化这个过程。今天你仍然可以使用这个方法，但 Git 和 GitHub 的广泛使用已经改变了人们获取和更新脚本的方式。现在，最主流的分发 Vim 脚本的方式是利用 GitHub，而用户则使用包管理器来调用 Git 从 GitHub（或类似的 Git 库）获取和更新脚本。

# vim8 的新功能
- 软件包的支持（`:help packages`）
- 异步任务的支持（`:help channel`、`:help job`、`:help timers`）
- 终端支持（`:help terminal`）

# vim 的软件包
vim 的目录结构比较Unix风格，一个功能用到的文件可能会分散在多个目录下。就像传统 Unix 上 Vim 的文件可能分散在 /usr/bin、/usr/share/man、/usr/share/vim 等目录下一样，一个 Vim 的插件（严格来讲，应该叫包）通常也会分散在多个目录下：
- 插件的主体一般放在`plugin`目录下
- 插件的帮助文件在`doc`目录下
- 有些插件只对某些文件有效，会有文件放在`ftplugin`目录下
- 有些插件有自己的文件类型检测规则，会有文件放在`ftdetect`目录下
- 有些插件有特殊的语法加亮，会有文件放在`syntax`目录下

以前安装插件，一般是一次性安装后就不管了。安装过程基本上就是到 .vim 目录（Windows 上是 vimfiles 目录）下，解出压缩包的内容，然后执行 `vim -c 'helptags doc|q'` 生成帮助文件的索引。到了“互联网式更新”的年代，这种方式就显得落伍了。尤其糟糕的地方在于，它是按文件类型来组织目录的，而不是按相关性，这就没法用 Git 来管理了。

后来出现了一些包管理器，它们的基本模式都是相通的：每个包有自己的目录，然后这些目录会被加到 Vim 的运行时路径（`runtimepath`）选项里。最早的 `runtimepath` 较为简单，在 Unix 上缺省为：
- $HOME/.vim
- $VIM/vimfiles
- $VIMRUNTIME
- $VIM/vimfiles/after
- $HOME/.vim/after

而在有了包管理器之后，runtimepath 就会非常复杂，每个包都会增加一个自己的目录进去。但是，好处也是非常明显的，包的管理变得非常方便。从Vim 8开始，Vim官方也采用了类似的体系。

Vim 会在用户的配置目录（Unix 下是 $HOME/.vim ，Windows 下是 $HOME/vimfiles ）下识别名字叫 pack 的目录，并在这个目录的子目录的 start 和 opt 目录下寻找包的目录。

对于下面的.vim目录：
```
.
├── colors
├── doc
├── pack
│   ├── minpac
│   │   ├── opt
│   │   │   ├── minpac
│   │   │   ├── vim-airline
│   │   │   └── vimcdoc
│   │   └── start
│   │       ├── VimExplorer
│   │       ├── asyncrun.vim
│   │       ├── fzf.vim
│   │       ├── gruvbox
│   │       ├── killersheep
│   │       ├── nerdcommenter
│   │       ├── nerdtree
│   │       ├── tagbar
│   │       ├── undotree
│   │       ├── vim-fugitive
│   │       ├── vim-matrix-screensaver
│   │       ├── vim-rainbow
│   │       ├── vim-repeat
│   │       ├── vim-rhubarb
│   │       └── vim-surround
│   └── my
│       ├── opt
│       │   ├── YouCompleteMe
│       │   ├── ale
│       │   ├── clang_complete
│       │   ├── cvsmenu
│       │   └── syntastic
│       └── start
│           ├── vim-gitgutter
│           └── ycmconf
├── plugin
├── syntax
└── undodir
```
可以看到，pack 目录下有 `minpac` 和 `my` 两个子目录（这些名字 Vim 不关心），每个目录下面又有 `opt` 和 `start` 两个子目录，再下面就是每个包自己的目录了，`里面又可以有自己的一套 colors、doc、plugin 这样的子目录`，这样就方便管理了。Vim 8 在`启动`时会`加载所有 pack/*/start 下面的包`，而用户可以用 `:packadd` 命令来加载某个 `opt` 目录下的包，如 `:packadd vimcdoc` 命令可加载 `vimcdoc` 包，来显示中文帮助信息。

有了上面这样的目录结构，用户要自己安装、管理包就方便多了。不过，我们还是推荐使用一个包管理器。包管理器可以带来下面的好处：
+ 可以在~/.vimrc中来配置需要安装的软件包
+ 自动化安装、升级、卸载，包括帮助文件的索引生成


# vim8 的最小包管理器 minpac
minpac是vim8最小的包管理器，这个minpac使用了vim8的`packages`特性和`jobs`特性。

首先，通过下面的命令下载minpac插件到~/.vim/pack/minpac/opt/minpac
```
git clone https://github.com/k-takata/minpac.git \
    ~/.vim/pack/minpac/opt/minpac
```

然后，简单配置~/.vimrc文件来使用minpac插件，在vimrc中添加下面的代码
```
if exists('*minpac#init')
  " Minpac is loaded.
  call minpac#init()
  call minpac#add('k-takata/minpac', {'type': 'opt'})

  " Other plugins
endif

if has('eval')
  " Minpac commands
  command! PackUpdate packadd minpac | source $MYVIMRC | call minpac#update('', {'do': 'call minpac#status()'})
  command! PackClean  packadd minpac | source $MYVIMRC | call minpac#clean()
  command! PackStatus packadd minpac | source $MYVIMRC | call minpac#status()
endif
```

下面就可以使用下面的命令了
- `PackUpdate`  安装插件（在.vimrc文件中的`" Other plugins`下面加入要安装的插件，例如`call minpac#add('tpope/vim-eunuch')`，然后运行`:PackUpdate`命令）
  
- `PackStatus`  查看minpac的状态窗口（直接运行`:PackStatus`会出现状态窗口）
  
- `PackClean`   删除插件（在.vimrc中将`" Other plugins`下面的插件对应的行删除，然后执行`:PackClean`即可）



