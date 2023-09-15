### 前言
Linux登录密码忘了，怎么重置呢？
### 第一步、进入恢复模式
重启系统，在进度条读完前，长按shift进入，GRUB系统引导界面。键盘上下键选择`Advanced options for Ubuntu`，按回车进选择界面，接着键盘上下键选中recovery mode项。按键盘上的`e`键进入编辑启动界面。
### 第二步、修改启动脚本
找到文本中的 `ro recovery nomodeset`，删除`recovery nomodeset`,并在该行最后写上`quiet splash rw init=/bin/bash`，按键盘F10键即可以root用户进入命令行界面。
### 第三步、查询用户名
使用命令
`cat /etc/passwd`
即可查看系统中的所有用户，找到你忘记密码的那个账户名xiaoyu。
### 第四步、重置密码
使用命令`passwd xiaoyu`即可重置用户`xiaoyu`的密码，连续输入两次即可确认使用该新密码。最后重启系统即可使用新密码进入。