# vim的安装
## redhat 和 centos
一般来说，centos和redhat系统上安装的vim都是最小功能版本，对于很多功能都是不支持的，但是这些功能可能对开发来说是非常重要的，比如python3的支持。  

敲击下面的命令来查看目前系统vim的版本情况：
```
vim --version
```
可以看到vim的版本号、以及支持的特性等，`+`表示支持,`-`表示不支持。

可以通过下面的命令在centos和redhat系统上查看vim的安装情况：
```
yum list installed | grep vim
```

如果只显示`vim-minimal.x86_64...`这样的结果，则说明安装的vim只有最基本的功能。此时，可以使用下面的命令来安装增强版本的vim：
```
sudo yum install vim-enhanced
```
或者，直接安装功能更加强大(自带剪切板支持)的图形界面版本的vim：
```
sudo yum install vim-X11
```

## debian 和 ubuntu
在ubuntu和debian上也有着不同版本的vim：
```
vim-tiny     //最小功能的vim
vim-nox      //有较全文本界面的vim
vim-athena   //适用于老的X-window界面
vim-gtk      //如果使用KDE桌面的话，建议
vim-gtk3     //如果使用GNOME桌面，建议
vim-gnome    //如果使用GNOME桌面，建议
```
可以使用下面的命令来查看已经安装的vim的版本：
```
apt list --installed | grep vim
```

## 手工源码编译vim
手工编译可以编译出具有特定特性的vim，因为vim在编译的时候有很多的编译配置选项，可以通过下面命令来查看有那些选项：
```
./configure --help
```

我编译的时候一般会使用下面的编译参数：
```
./configure --enable-pythoninterp --enable-python3interp --enable-gui=auto
make -j
sudo make install
```
其中，分别表示支持python2、python3（目前很多插件都要求python3,python2已经停止支持）、图形界面。其中的auto可以换成其他的图形界面，比如`gtk3`。经过上面的步骤，vim就被安装到`/usr/local/bin`目录下了，可以使用`where`或`which`命令来查看。

