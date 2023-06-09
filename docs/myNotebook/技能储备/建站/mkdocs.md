搭建mkdocs

> 从简书搬家到mkdocs，由于简书为了优化存储速度将原始的png转化为了webp,我为了还原png与gif,写了一系列的脚本，将webp转为png

### 安装mkdocs

```shell
pip3 install mkdocs
```

创建一个Demo

```shell
mkdocs new my-notebook
```

会在当前目录下创建一个项目文件夹my-notebook;

```
.
├── docs
│   └── index.md
└── mkdocs.yml
```

配置文件mkdocs.yml，将项目名称改为My NoteBook

```yml
site_name: My NoteBook
```

### 启动服务

```
mkdocs serve
```

### 使用Material美化mkdocs

```shell
pip3 install mkdocs-material
```

注意：如果要自定义logo文件，则需要将文件存在`docs`文件夹下

最终配置文件：

```yml
# Project information
site_name: Xiaoyu NoteBook
site_author: Matrix Swift

# Copyright
copyright: Copyright &copy; 2015 - 2023 Matrix Swift

# Configuration
theme:
  name: material
  favicon: assets/water.png
  icon:  
    logo: material/cat
  features:
    - search.suggest
    - search.highlight
    - navigation.footer
    - navigation.tabs
    - navigation.sections
    - content.tabs.link
    - content.code.copy
  palette:
    - scheme: default
      primary: green
      accent: light green
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - scheme: slate
      primary: teal
      accent: teal
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
#Customization
extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/tanyusheng
    - icon: fontawesome/brands/zhihu
      link: https://www.zhihu.com/people/tan-yu-sheng-63
    - icon: fontawesome/brands/bilibili
      link: https://space.bilibili.com/22048069
    - icon: fontawesome/brands/youtube
      link: https://www.youtube.com/channel/UCX0mNfkFoEHL2HNrfGK2gMA
    

# Extensions
markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.superfences

plugins:
  - search

# Page tree
nav:
  - Home: index.md
  
  - Django: 
    - Django框架学习笔记: 
      - myNotebook/Django/Django框架学习笔记/01_认识Django.md
      - myNotebook/Django/Django框架学习笔记/02_URL路由.md
      - myNotebook/Django/Django框架学习笔记/03_Templates模板.md
      - myNotebook/Django/Django框架学习笔记/04_URL跳转与多app环境.md
      - myNotebook/Django/Django框架学习笔记/05_给URL命名.md
      - myNotebook/Django/Django框架学习笔记/06_模板语言DTL.md
      - myNotebook/Django/Django框架学习笔记/07_模板的抽象与继承.md
  - Linux: 
    - Linux学习笔记:
      - myNotebook/Linux学习/Linux学习笔记/01_学会使用Vim.md
      - myNotebook/Linux学习/Linux学习笔记/02_Linux系统基础操作.md
      - myNotebook/Linux学习/Linux学习笔记/03_文件和文本的查看.md
      - myNotebook/Linux学习/Linux学习笔记/04_目录操作.md
      - myNotebook/Linux学习/Linux学习笔记/05_打包压缩和解压缩.md
      - myNotebook/Linux学习/Linux学习笔记/06_用户与用户组.md
      - myNotebook/Linux学习/Linux学习笔记/07_文件与目录的权限.md
      - myNotebook/Linux学习/Linux学习笔记/08_网络管理.md
      - myNotebook/Linux学习/Linux学习笔记/09_软件包管理.md
    - 在Ubuntu上安装搜狗输入法: myNotebook/Linux学习/在Ubuntu上安装搜狗输入法.md
    - 虚拟机里的Ubuntu忘记了密码: myNotebook/Linux学习/虚拟机里的Ubuntu忘记了密码,怎么修改密码.md
    - 让你的Linux终端开机欢迎语更好看: myNotebook/Linux学习/让你的Linux终端开机欢迎语更好看.md
    - Linux基础知识总结: myNotebook/Linux学习/Linux基础知识总结.md
    - Synergy多台电脑共享键鼠: myNotebook/Linux学习/Synergy多台电脑共享键鼠.md
    
  - MySQL:
      - myNotebook/Mysql基础/01_数据库的安装与Navicat连接.md
      - myNotebook/Mysql基础/02_数据库的基本操作.md
      - myNotebook/Mysql基础/03_SQL基础查询.md
      - myNotebook/Mysql基础/04_聚合函数与分组嵌套查询.md
      - myNotebook/Mysql基础/05_连接查询.md
      - myNotebook/Mysql基础/06_Django连接MySQL.md
  - 前端: 
    - HTML:
      - HTML学习笔记(上): myNotebook/frontEnd/HTML学习笔记(上).md
      - HTML学习笔记(下): myNotebook/frontEnd/HTML学习笔记(下).md
    - JavaScript: 
      - JavaScript基础: myNotebook/frontEnd/JavaScript基础.md
      - JavaScript进阶: myNotebook/frontEnd/JavaScript进阶.md
    - CSS: 
      - CSS盒子模型: myNotebook/frontEnd/css布局:盒子模型与定位.md
      - CSS选择器: myNotebook/frontEnd/css布局:语法规范与选择器.md
  
  - Python:
    - Python基础:
      - myNotebook/Python学习笔记/Python基础/01_变量与数据类型.md
      - myNotebook/Python学习笔记/Python基础/02_运算符.md
      - myNotebook/Python学习笔记/Python基础/03_条件与选择循环.md
      - myNotebook/Python学习笔记/Python基础/04_列表与元组.md
      - myNotebook/Python学习笔记/Python基础/05_集合与字典.md
      - myNotebook/Python学习笔记/Python基础/06_日期与时间.md
      - myNotebook/Python学习笔记/Python基础/07_函数.md
      - myNotebook/Python学习笔记/Python基础/08_异常处理.md
      - myNotebook/Python学习笔记/Python基础/09_字符串.md
      - myNotebook/Python学习笔记/Python基础/10_正则表达式上.md
      - myNotebook/Python学习笔记/Python基础/11_正则表达式下.md
      - myNotebook/Python学习笔记/Python基础/12_文件和目录.md
      - myNotebook/Python学习笔记/Python基础/13_图形化界面Tkinter.md
    - Python面向对象:
      - myNotebook/Python学习笔记/Python面向对象/01_理解类和对象.md
      - myNotebook/Python学习笔记/Python面向对象/02_用Python写一个身份证校验系统.md
      - myNotebook/Python学习笔记/Python面向对象/03_用Python自动统计列表中男生女生的人数.md
      - myNotebook/Python学习笔记/Python面向对象/04_Python中的实例方法类方法静态方法的区别.md
      - myNotebook/Python学习笔记/Python面向对象/05_Python中的属性与私有字段.md
      - myNotebook/Python学习笔记/Python面向对象/06_Python中类的组合与创建.md
      - myNotebook/Python学习笔记/Python面向对象/07_Python调用终端模拟红绿灯.md
      - myNotebook/Python学习笔记/Python面向对象/08_Python方法中的继承重写和扩展.md
      - myNotebook/Python学习笔记/Python面向对象/09_用Python实现俄罗斯方块的基本变换.md
      - myNotebook/Python学习笔记/Python面向对象/10_Python中的私有属性.md
      - myNotebook/Python学习笔记/Python面向对象/11_Python中的多继承.md
      - myNotebook/Python学习笔记/Python面向对象/12_Python中的抽象类和抽象方法.md
      - myNotebook/Python学习笔记/Python面向对象/13_学生教师信息管理系统step01.md
      - myNotebook/Python学习笔记/Python面向对象/14_学生教师信息管理系统step02.md
      - myNotebook/Python学习笔记/Python面向对象/15_学生教师信息管理系统step03.md
      - myNotebook/Python学习笔记/Python面向对象/16_学生教师信息管理系统step04.md
      - myNotebook/Python学习笔记/Python面向对象/17_学生教师信息管理系统step05.md
    - Tkinter项目实战: 
      - myNotebook/Python学习笔记/Tkiner项目实战/01_登录窗体的实现.md
      - myNotebook/Python学习笔记/Tkiner项目实战/02_主窗体的界面设计与实现.md
      - myNotebook/Python学习笔记/Tkiner项目实战/03_实现信息查询功能.md
      - myNotebook/Python学习笔记/Tkiner项目实战/04_实现学生明细窗体GUI设计.md
      - myNotebook/Python学习笔记/Tkiner项目实战/05_填充学生明细信息.md
      - myNotebook/Python学习笔记/Tkiner项目实战/06_添加学生信息.md
      - myNotebook/Python学习笔记/Tkiner项目实战/07_学生信息的修改删除和保存.md
      - myNotebook/Python学习笔记/Tkiner项目实战/08_实现修改密码的功能.md
      - myNotebook/Python学习笔记/Tkiner项目实战/09_项目总结.md
  - 树莓派:
      - myNotebook/树莓派/树莓派人脸识别实际应用:人脸识别门禁.md
      - myNotebook/树莓派/树莓派如何刷RetroPie，制作一个复古游戏机.md
      - myNotebook/树莓派/树莓派如何开启SSH服务.md
      - myNotebook/树莓派/树莓派安装Samba服务，实现家庭文件共享.md
      - myNotebook/树莓派/树莓派搭建网络视频实时监控系统.md
      - myNotebook/树莓派/树莓派调用百度人脸识别API实现人脸识别.md
      - myNotebook/树莓派/物联网家庭环境监控系统项目介绍.md
      - myNotebook/树莓派/用Terminal给SD卡写入img镜像的方法.md
      - myNotebook/树莓派/给树莓派设置静态IP.md
  - 物联网:
      - myNotebook/物联网/Arduino通过ESP8266连接机智云物联网平台.md
      
  - 计算机课程笔记: 
    - 操作系统:
      - myNotebook/计算机课程笔记/操作系统/01操作系统的概念和定义.md
      - myNotebook/计算机课程笔记/操作系统/02操作系统进程与线程.md

  - 技能储备:
    - 算法与数据结构:
      - myNotebook/技能储备/算法与数据结构/ACM模式的输入输出处理(Java篇).md
    - Android: 
      - myNotebook/技能储备/Android/Android gradle学习笔记.md
      - myNotebook/技能储备/Android/Android刷机.md
      - myNotebook/技能储备/Android/机智云Android项目导入AS.md 
    - git:
      - myNotebook/技能储备/Git/学习并熟练掌握git.md
    - 效率工具:
      - myNotebook/技能储备/效率工具/学会使用oh-my-zsh.md
    - 网络技术:
      - myNotebook/技能储备/网络技术/使用frp进行内网穿透.md
      - myNotebook/技能储备/网络技术/如何查找当前网段下的所有ip地址.md
    - 建站:
      - mkdocs: myNotebook/技能储备/建站/mkdocs.md
    - Markdown:
      - myNotebook/技能储备/Markdown/让你的公众号Markdown排版更好看.md
      - myNotebook/技能储备/Markdown/马克飞象如何自定义代码高亮风格.md
    - 数据可视化:
      - myNotebook/技能储备/数据可视化/01_matplotlib教程.md
      - myNotebook/技能储备/数据可视化/02_matplotlib教程.md
      - myNotebook/技能储备/数据可视化/matplotlib学习笔记.md
    - Mac技巧:  
      - myNotebook/技能储备/Mac技巧/Mac记住密码与IP快速ssh连接服务器.md
    - shell:
      - myNotebook/技能储备/shell/shell2giforpng.md
      - myNotebook/技能储备/shell/shell学习笔记.md
  - 随笔: 
    - typora: myNotebook/随笔/typora使用技巧.md
    - 开源协议: myNotebook/随笔/开源协议比较.md
```

