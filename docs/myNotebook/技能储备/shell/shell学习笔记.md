## Shell学习笔记

### shebang

写shell脚本要遵循一定的规范，在shell脚本中，文本文件第一行的前两个字符`#!`成为`shebang`，在Unix系统中程序会分析`shebang`后面的内容，作为解释器的指令。例如：

* 以`#! /bin/bash`开头的文件，脚本执行的时候，默认用当前shell去解释脚本；
* 以`!# /usr/bin/python`开头代表指定Python解释器去执行。
* 以`!# /usr/bin/env 解释器名称` 是一种在不同平台上都能找到解释器的办法

### 基本理念

如果文件本身未添加执行权限，可以使用`bash script.sh`或者`sh script.sh`来直接执行；
shell是一种弱类型语言，不用主动声明数据类型；
查看当前linux版本支持多少种shell脚本，可以使用命令`cat /etc/shells`查看

### history

```shell
# 查看当前系统存储的history长度
echo $HISTSIZE

# 调用指定编号历史记录的命令
! 历史id

# 执行上次相同的命令
！！
```

### 变量

在shell中，变量与值之间不得有空格；弱类型语言不需要声明变量类型，默认变量的值为字符串，单引号变量不识别特殊符号，双引号变量识别特殊符号；
输出变量可以使用$变量名的形式

```shell
name="xiaoyu"
echo $name
```

实际上，`$`也是简写，对变量 引用完整的写法是：`${变量名}`

```shell
name="xiaoyu"
echo ${name}
```

### 获取文件的前后缀

```shell
#!/bin/bash
 
filename=rongtao.tar.gz
 
echo "${filename%%.*}"
 
echo "${filename%.*}"
 
echo "${filename#*.}"
 
echo "${filename##*.}"
 
#运行结果：
#  sh extension.sh 
#rongtao
#rongtao.tar
#tar.gz
#gz

```

