### 1.软件准备

[Ubuntu下载地址](https://wiki.n.miui.com/download/attachments/425329668/synergy-v1.8.8-stable-Linux-x86_64.deb?version=1&modificationDate=1597632979000&api=v2)

[windows下载地址](https://wiki.n.miui.com/download/attachments/425329668/synergy-v1.8.8-stable-Windows-x64.msi?version=1&modificationDate=1597633069000&api=v2)

### 2.Ubuntu中使用命令安装deb文件。

```shell
sudo dpkg -i synergy-v1.8.8-stable-Linux-x86_64.deb
```

如果报错依赖XX但是未安装，更新安装缺少的包。

```shell
sudo apt update
```

```sh
sudo apt --fix-broken install
```

安装所缺的依赖包

```sh
sudo apt install xxx
```



### 3.设置server端和client端，实现用server的鼠标键盘控制server和client。

   两台电脑都打开Synergy

3.1其中server的软件中勾选Server，在交互配置里点“设置服务端”，拖动右上角的显示器按钮到网格中，此时显示器的布局就对应控制时的位置。双击未命名的显示器图标（代表客户端的显示器），将客户端软件显示的屏幕名打进去，确认。服务器端准备完毕后点开始，显示synergy正在运行并输出日志。

3.2client的软件中勾选Client。在服务端IP中输入server软件中显示的IP地址。client软件点开始，显示synergy正在运行并输出日志。

> 注：

如果出现问题：client显示 failed to connect to server:Timed out。server显示client connection may not be secure.failed to accept secure socket.

解决方案：软件窗口菜单栏——编辑——设置。**查看Network Security中的Use SSL encryption是否勾选，server和client需要保持一致，客户端与服务端加密由于不加密要保持一致**。