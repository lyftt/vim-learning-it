# 行首行尾的跳转命令
- `0`   跳转到行首
- `$`   跳转到行尾
- `^`   跳转到行首的第一个非空白字符

# 以单词为单位的跳转
- `b/B`  跳过一个单词至上一个单词词首（左移）
- `w/W`  跳过一个单词至下一个单词词首（右移）
- `e/E`  跳过一个单词至下一个单词词尾（右移）
- `5w`   表示跳过5个单词
- `5e`   表示跳过5个单词
注意，这些命令之前都可以使用数字，来表示要跳转的单词数量。`/`表示或，敲命令的时候不需要敲。


大小写命令（比如b/B）的区别在于，它们认为的单词的标准不同：
+ 小写命令：认为单词是由字母、数字、下划线组成，和编程语言中的标识符规则相似；
+ 大写命令：认为非空格字符都是单词；

# 句和段落跳转
+ `(` 和 `)`跳到上一句和下一句
+ `{` 和 `}`跳到上一段和下一段
+ `5(`   向上跳5句
+ `5{`   向下跳5句

# `f`和`t`命令
它们并不是完整的命令，都需要在后面跟上一个字符。
- `f>`  行内查找后跟的字符`>`(实际可以为任何其他字符)的位置，并移动到这个字符位置，包含这个字符;
- `F>`  行内反向查找
- `t>`  行内查找后跟的字符`>`的下一个位置，并移动到这个字符位置的前一个位置（即减1），即不包含这个字符;
- `T>`  行内反向查找
- `2f>` 行内找后跟的字符`>`出现的第二个位置，并移动到这个字符位置;
- `3t<` 行内找后跟的字符`<`出现的第三个位置，并移动到这个位置的前一个位置（即减1）;

所以，其实可以在`f/t/F/T`命令之前使用数字，来表示要查找第几个。

# 修改命令`c`
`c`是修改命令，含义是删除部分内容并进入插入模式。它后面可以接一个操作范围：
```
cw       删除当前光标所在单词，并进入插入模式
ciw      删除当前光标所在单词，并进入插入模式
c$       删除当前光标位置到本行结尾处的内容，并进入插入模式（还记得$表示行尾吗？）
```

如果你想修改下面语句中括号内的内容，应该怎么修改？
```
if(ptr->next != NULL)
```
```
ct)   表示删除部分内容，然后进入插入模式，删除的内容为当前光标到行内下一个）出现的位置，这个）不会删除;

cf)   表示删除部分内容，然后进入插入模式，删除的内容为当前光标到行内下一个）出现的位置，这个）也会删除;

所以修改上面的if括号中的内容的这种情况，应该使用ct)
```

注意，`c`命令删除的内容都保存在vim的寄存器中，如果没有指定特定的寄存器，则默认在无名寄存器`""`中。所以可以配合寄存器来实现剪切操作。

# 跳转文本开头和结尾
- `gg`   跳转到文本开头
- `G`    跳转到文本最后一行的第一个字符（不是最后一个字符）

# 文本修改命令
vim中通常使用`c`命令和`d`命令配合方向键来进行更改。`c`命令其实可以视为先执行`d`删除，然后执行`i`命令进入插入模式。vim中很多相近的功能会有不同的按键，大写键一般也会重载一个相近但是稍稍不同的含义。

```
dd  d命令加动作来进行删除（dd删除整行）
d$  删除到行尾
D   大写的D相当于d$命令

cc  c命令加动作来进行修改（cc修改整行）
c$  删除到行尾然后进入插入模式
C   大写的C相当于c$命令

s   相当于cl命令，删除一个字符，然后进入插入模式
S   相当于cc，替换整行内容

i   在当前字符前面进入插入模式
I   相当于^i，将光标移动到行首非空白字符，然后进入插入模式

a   在当前字符之后进入插入模式
A   相当于$a，即把光标移动到行尾，然后进入插入模式

o   在当前行下方插入一个新行，然后在改行进入插入模式
O   在当前行上方插入一个新行，然后在该行进入插入模式

r   替换光标下的字符
R   进入替换模式，每次按键替换一个字符

u   撤销最近一个修改动作
U   撤销当前行上所有修改

```

# 超强的文本对象选择(最重要，coding最常用)
这部分内容和编程息息相关，是用vim编写程序的强大助力。

可以使用v命令来选择一块区域并对这块区域进行修改。但是，其实也可以通过在`c`、`d`、`v`、`y`命令之后加上一些动作，来对指定的文本进行操作，比如修改、删除。这些动作就是`a`和`i`。
```
a的含义：可以理解为英文单词all，表示全部；
i的含义：可以理解为英文单词inner，表示内部；
```

用下面的例子就明白了：
```
if(message == "hello world")
{
    if(message.size() == 11)
    {
        return true;
    }
    else
    {
        return false;
    }
}
```
假设，目前vim的光标在最外面的if语句的`hello world`的`e`上，则使用下面命令的结果为：
```
dw     理解为delete a word，则会删除'ello '，即之后的结果为if(message == "hworld")，将ello和后面的一个空格都删除了；含义是删除当前光标开始的一个单词；

diw    理解为delete inside word，则会删除'hello'，即之后的结果为if(message == " world")；含义是当前在单词内部，删除当前所在单词；

daw    理解为delete a word，则会删除'hello '，即之后的结果为if(message == "world")，含义是删除一整个单词；

diW    会删除'"hello'，即之后的结果为if(message == world"),大写的W认为非空格字符都是单词，所以'"hello'被视为一个单词，然后被删除；

daW    会删除'"hello '，即之后的结果为if(message == world")，其实结果和diW一样；

di"    会删除""内部的所有字符，即之后的结果为if(message == "");

da"    会删除""内部的所有字符，连带""也一起删除，即之后的结果为if(message ==)

di) 或 di(  会删除括号()内的所有字符，即之后的结果为if();

da) 或 da(  会删除括号()内的所有字符，包括括号()，所以之后的结果为if。

```
上面展示了`d`和`()`、`""`的搭配使用，其实还可以和`[]`、`{}`搭配起来用，那么写代码就非常方便了。除了`d`以外，还有`c`、`v`、`y`等命令，都可以这样使用。

如下面的例子：
```
if(message == "hello world")
{
    if(message.size() == 11)
    {
        return true;
    }
    else
    {
        return false;
    }
}
```
假设光标在`return true`上，使用下面的命令：
`ci}` 或 `ci{` 会将`return true`所在的{}中的内容全部删除，然后进入编辑模式，即结果为
```
if(message == "hello world")
{
    if(message.size() == 11)
    {
        |
    }
    else
    {
        return false;
    }
}
```

`c2i{` 或 `c2i}`会将`return true`所在的{}的外面一层的{}中的内容全部删除，然后进入编辑模式，即结果为
```
if(message == "hello world")
{
    |
}
```

# 翻页
`<C - F>`  下一页，C表示ctrl键

`<C - B>`  上一页

`<C - U>`  上半页

`<C - D>`  下半页

`数字 + G`   跳转到指定行

`数字 + |`   跳转到指定的列

例如：
- `5G37|`   表示跳转到5行37列
- `vim -c "normal 5G12|" main.cpp`  表示打开文件同时跳转到5行12列

# 滚动屏幕
`set scrolloff=1`  设置滚轮的粒度(滚动屏幕操作都受这个设置的影响)

`<C - E>` 滚轮下滚（不需要鼠标了）  
`<C - Y>`  滚轮上滚


`zt`  把当前行滚动到屏幕顶部（注意是滚动当前行，光标坐在位置不会改变）  
`zz`  把当前行滚动到屏幕中部  
`zb`  把当前行滚动到屏幕底部  


# 重复键
很多命令可能要敲好几个键，如果每次都要敲这么多的命令，则会相当麻烦，可以通过命令录制、键映射、自定义脚本等复杂的操作来解决外，vim还提供了一些`重复键`，来提高效率：


`;` 重复最近的字符查找（`f`、`t`命令等）


`,` 重复最近的字符查找操作，反方向


`n` 重复最近的字符串查找操作(`/`和`?`命令)


`N` 重复最近的字符串查找操作(`/`和`?`命令)，反方向


`.` 重复执行最近的修改操作(`c`、`d`、`i`、`a`、`o`等都可以)

