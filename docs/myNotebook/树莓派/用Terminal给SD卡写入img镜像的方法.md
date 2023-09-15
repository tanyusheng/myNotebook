### 前言
给sd卡写入官方镜像，在windows上要用Win32 Disk Image,因为我用的是Mac系统切来切去有点麻烦，要是直接在Mac上写就方便多了。

> 官方也提供了专门的烧录工具Raspberry Pi Imager,使用命令行也会很方便；

对于要写入的SD卡，我们通常需要对它进行格式化，可以用格式化工具SD Card Formatter,这款格式化工具可以从SD官网[下载](https://www.sdcard.org/downloads/formatter/)
![01](localpicbed/用Terminal给SD卡写入img镜像的方法.assets/01.png)

选中要操作的SD卡，点击Format即可；

### 操作步骤

##### 1.进入目录

插入要写入的sd卡，进入Mac上存放img镜像文件的目录，比如我的就是放在Desktop上的raspberryiso文件夹，那么terminal的命令就是:

```
cd ~/desktop/rasiberryiso
```
##### 2.列出目前系统上的所有磁盘;

```
diskutil list
```
在terminal里找到你要写入的磁盘的编号；
![02](localpicbed/用Terminal给SD卡写入img镜像的方法.assets/02.png)

##### 3.推出此磁盘
```
diskutil unmountDisk /dev/<disk#> 
```
(<disk#>换成你要写入的磁盘编号）
![03](localpicbed/用Terminal给SD卡写入img镜像的方法.assets/03.png)

##### 4.用dd命令将树莓派系统镜像写入SD卡
```
    sudo dd bs=1m if=<your image file name>.img of=/dev/<disk#> 
```
(<your image file name>换成要你写入镜像的文件名)

输入这个命令后系统会提示你输入密码，此时输入你电脑账户的密码即可。
![04](localpicbed/用Terminal给SD卡写入img镜像的方法.assets/04.png)

#### 建议

终端写入镜像会花费较长时间，没有进度条请耐心等待，请不要急着关闭相应“终端”窗口，我给树莓派烧录一个系统镜像花了一个多小时才好，一定要有耐心，这一点很重要。

##### 题外话

> 某天，我在使用命令`cat /proc/cpuinfo`时看到树莓派的内核信息为armhf(支持hard float的32位arm)。
>
> 去[官网](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/?ref=blog.mythsman.com)查询了一下，树莓派4b的CPU是`Broadcom BCM2711, Quad core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5GHz`应该是arm64的内核才对。
>
> 又查了一波资料发现，树莓派内核信息与系统镜像的版本有关；如果使用的早期系统镜像内核就是armhf,如果想使用arm64内核，需要烧录新版的系统镜像。
>
> 可以在这个[reddit](https://www.reddit.com/r/raspberry_pi/comments/gs6omd/raspberry_pi_os_for_arm64_finally_released/?ref=blog.mythsman.com)文章中看到，arm64位的系统比armhf更能发挥出64位CPU的树莓派的CPU性能。
>
> 故推荐大家使用arm64内核的镜像，关于树莓派的镜像文件去哪下载，可以选择树莓派官网，或者清华的镜像站[tuna](https://mirrors.tuna.tsinghua.edu.cn/raspberry-pi-os-images/raspios_arm64/images/)有下载提供树莓派镜像文件；