---
title: vim备忘
date: 2021-09-28 23:11:07
---
# Vim操作方式备忘
---
Command模式

分屏

:vs (vertical split), :sp(split)

全局替换

:% s/带替换内容/替换为/g

---
Visual模式

使用 v 进入

使用 V 选择行

使用 ctrl+v 方块选择

---
编辑模式

ctrl+h 删除上一个字符

ctrl+w 删除上一个单词

ctrl+u 删除当前行

(在终端下 ctrl+a 移动到行首  ctrl+e 移动到行尾 ctrl+b 向前移动  ctrl+f 向后移动)


---
快速切换从insert模式去normal模式

除了esc还可以ctrl+c(有可能中断其他插件)还可以ctrl+[

快速切换从noraml模式去insert模式

使用 gi 快速跳转到你最后一次编辑的地方并进入插入模式

---

### 行间搜索移动

- 使用f{char}可以移动到 char 字符上，t{char}移动到 char 的前一个字符
- 如果第一次没有搜到，可以用分号(;)/逗号(,)继续搜该行的上一个/下一个
- 使用F表示反过来搜索前面的字符

### vim的水平移动

- 0 移动到行首第一个字符，^ 移动到第一个非空白字符
- $ 移动到行尾，g_ 移动到行尾非空白字符

### vim的页面移动

- gg/G 移动到文件开头或者结尾， 使用 ctrl+o 快速返回
- 使用 H/M/L 跳转到屏幕的 开头（Head） 中间（Middle） 结尾（Lower）
- Ctrl+u ctrl+f 上下翻页 （upword/forward） zz把屏幕置为中间

### vim的快速修改

- 常用有三个，r(replace), c(change), s(substitute)
- 在normal模式中 r 可以替换一个字符， s 替换并插入一个字符
- 使用c配合文本对象，我们可以快速进行修改

### vim的查询

- 使用/或者?来进行向前或者反向的搜索
- 使用n/N跳转到下一个或者上一个匹配
- 使用*/#进行当前光标单词的前向或者后项匹配


### vim的替换命令

substitute命令允许我们查找并且替换掉文本，并且支持正则

- :[range] s[ubstitute]/{pattern}/{string}/[flags]
- range 表示范围 比如： 10,20 表示 10-20行，  % 表示全部
- pattern是要替换的模式，string是替换后的文本

> 替换标志位
	g(global) 表示全局范围内执行
	c(comfirm) 表示确认， 可以确认或者拒绝修改
	n(number) 报告匹配到的次数而不替换，可以用来查询匹配次数



## vim的多文件操作

### Buffer Window Tab

- Buffer是指打开的一个文件的内存缓冲区
- 窗口是Buffer可视化的分割区域
- Tab 可以组织窗口为一个工作 区

### Buffer - 什么是缓冲区

- vim 打开一个文件后会加载文件内容到缓冲区
- 之后的修改都是针对内存中的缓冲区，并不会直接保存到文件里
- 知道我们执行： w(write)的时候才会吧修改内容写入到文件里

### 如何进行buffer切换

- 使用:ls 会列举当前缓冲区，然后使用:b n跳转到第n个缓冲区
- :bpre :bnext :bfirst :blast
- 或者:b buffer_name加上tab补全来跳转

(在编辑的过程中可以使用`:e 文件名`来打开新的文件 edit)

### Window - 窗口是可视化的分割区域

- 一个缓冲区可以分割成多个窗口，每个窗口也可以打开不同的缓冲区
- <ctrl+w>s 水平分割, <ctrl+w>v 垂直分割，或者:sp和:vs
- 每个窗口可以继续无限分割

### 如何切换窗口

切换窗口都是使用`ctrl+w`(window)作为前缀


|按键|说明|
|---|---|
|<ctrl+w>w|在窗口间循环|
|<ctrl+w>h|切换到左边的窗口|
|<ctrl+w>j|切换到下边的窗口|
|<ctrl+w>k|切换到上边的窗口|
|<ctrl+w>l|切换到右边的窗口|
|<ctrl+w>H|移动当前窗口到左边|
|<ctrl+w>J|移动当前窗口到下面|
|<ctrl+w>K|移动当前窗口到上面|
|<ctrl+w>L|移动当前窗口到右边|


### tab(标签页)，将窗口重组

tab是可以容纳一系列窗口的容器（:h tabpage）

## vim中的文本对象

### 文本对象的操作方式

- [number]<command>[text object]
- number 表示次数，command 是命令，d(delete),c(change),y(yank) 
- text object 是要操作的对象，比如单词w，句子s，段落p


## vim的寄存器

Vim不使用单一剪贴板进行剪贴，复制和粘贴，而是多组寄存器

- 通过 `"{register}` 前缀可以指定寄存器，不指定默认用无名寄存器
- 比如使用 `"ayiw` 复制一个单词到寄存器a中，`"bdd` 删除当前行到寄存器b中
- （可以用:reg a 查看a寄存器的内容）
- 使用`"ap`粘贴a寄存器中的内容


### 其他常见寄存器

除了有名寄存器a-z，vim中还有其他寄存器

- 复制专用寄存器 `"0` 使用y复制文本的同时会被拷贝到复制寄存器0
- 系统剪贴板 `"+` 可以在复制前加上`"+`复制到系统剪贴板 （需要使用 :echo has('clipboard') 输出1来确保可以被系统使用）
- 其他一些寄存器比如 `"%`当前文件名， `".`上次插入的文本

**可以使用`:set clipboard=unnamed`可以直接复制粘贴系统剪贴板的内容**


## Vim中的宏

宏的使用分为录制和回放

- vim使用q来录制，同时也是q来结束录制（normal模式）
- 使用q{register}选择要保存的寄存器，把录制的命令保存其中
- 使用@{register}回放寄存器中保存的一系列命令

(例如可以输入 :normal @a 对选中的部分回放宏)


## vim补全

vim中的补全方式


|命令|补全类型|
|---|---|
|&lt;ctrl-n&gt;|普通关键字|
|&lt;ctrl-x&gt;&lt;ctrl-n&gt;|当前缓冲区关键字|
|&lt;ctrl-x&gt;&lt;ctrl-i&gt;|包含文件关键字|
|&lt;ctrl-x&gt;&lt;ctrl-]&gt;|标签文件关键字|
|&lt;ctrl-x&gt;&lt;ctrl-k&gt;|字典查找|
|&lt;ctrl-x&gt;&lt;ctrl-l&gt;|整行补全|
|&lt;ctrl-x&gt;&lt;ctrl-f&gt;|文件名补全|
|&lt;ctrl-x&gt;&lt;ctrl-o&gt;|全能（Omni）补全|


## vim的映射

### 基本映射

基本映射指的是normal模式下的映射
- 使用map就可以实现映射。比如:map - x 然后按-就可以删除字符
- :map &lt;space&gt; viw 告诉vim按下空格的时候选中整个单词
- :map &lt;c-d&gt; dd 可以使用ctrl+d执行dd删除一行

### 模式映射
Vim常用模式Normal/Insert/Visual都可以定义映射
- 用nmap/imap/vmap定义映射只在normal/insert/visual模式下分别有效
- :vmap \ U 把在visual模式下按下`\`选中的文本变为大写
- 在insert模式下映射ctrl+d来删除一行  :imap <c-d> <Esc>ddi

### 递归与非递归映射
***map系列命令有递归的风险**
vim提供了非递归映射，这些命令不会递归解释
- 使用nnoremap/vnoremap/inoremap