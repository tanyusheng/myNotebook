## 前言

大多数情况下，用户下载都是通过浏览器这种单线程下载方式；树莓派作为远程下载机，将电影资源下载到硬盘中，并将资源在局域网内共享推流；

现在推荐一种多线程高速下载+局域网流媒体播放+可视化下载管理页面+局域网文件管理共享的技术实现方案；采用的工具有Aria2、AiraNg、Samba、MiniDLNA、Nginx;

## 二、安装Aria2

1. 安装软件包

```shell
sudo apt-get install aria2
```

2. 创建配置文件

```shell
mkdir -p ~/.config/aria2/
touch ~/.config/aria2/aria2.session
vim ~/.config/aria2/aria2.config
```

3. 添加配置信息

```config
# set your own path
dir=[yourpath]
disk-cache=32M
file-allocation=trunc
continue=true

max-concurrent-downloads=10

max-connection-per-server=16
min-split-size=10M
split=5
max-overall-download-limit=0
#max-download-limit=0
#max-overall-upload-limit=0
#max-upload-limit=0
disable-ipv6=false

save-session=/home/pi/.config/aria2/aria2.session
input-file=/home/pi/.config/aria2/aria2.session
save-session-interval=60


enable-rpc=true
rpc-allow-origin-all=true
rpc-listen-all=true
rpc-secret=secret
#event-poll=select
rpc-listen-port=6800


# for PT user please set to false
enable-dht=true
enable-dht6=true
enable-peer-exchange=true

# for increasing BT speed
listen-port=51413
#follow-torrent=true
#bt-max-peers=55
#dht-listen-port=6881-6999
#bt-enable-lpd=false
#bt-request-peer-speed-limit=50K
peer-id-prefix=-TR2770-
user-agent=Transmission/2.77
seed-ratio=0
#force-save=false
#bt-hash-check-seed=true
bt-seed-unverified=true
bt-save-metadata=true
bt-tracker=http://93.158.213.92:1337/announce,udp://151.80.120.114:2710/announce,udp://62.210.97.59:1337/announce,udp://188.241.58.209:6969/announce,udp://80.209.252.132:1337/announce,udp://208.83.20.20:6969/announce,udp://185.181.60.67:80/announce,udp://194.182.165.153:6969/announce,udp://37.235.174.46:2710/announce,udp://5.206.3.65:6969/announce,udp://89.234.156.205:451/announce,udp://92.223.105.178:6969/announce,udp://51.15.40.114:80/announce,udp://207.241.226.111:6969/announce,udp://176.113.71.60:6961/announce,udp://207.241.231.226:6969/announce
```

> 注意事项：系统配置文件路径无法识别`“~”`,请使用绝对路径；

配置完成后，使用命令启动Aria2服务：

```shell
sudo aria2c --conf-path=/home/pi/.config/aria2/aria2.config
```

启动完成后系统提升监听6800端口；这事我们先关闭这个服务，将启动方式添加到系统开机自启动服务中；

4. 创建系统服务文件

```shell
sudo nano /lib/systemd/system/aria2.service
```

5. 配置系统服务文件信息

```
[Unit]
Description=Aria2 Service
After=network.target

[Service]
User=pi    
ExecStart=/usr/bin/aria2c --conf-path=/home/pi/.config/aria2/aria2.config

[Install]
WantedBy=default.target
```

6. 重载服务并开启自启动

```shell
sudo systemctl daemon-reload
sudo systemctl enable aria2
sudo systemctl start aria2
sudo systemctl status aria2
```

不出意外的话，aria2已经实现了开机自动启动了；这时候多线程下载已经实现了，但是每次通过命令行控制过于麻烦，故配置一个前端的页面进行交互控制。可以使用AiraNg由html、css、JavaScript实现的针对Aria2的前端页面，启动这个服务需要通过web静态网站反向代理，这里推荐使用Nginx;

## 三、安装Nginx

1. 安装Nginx

```shell
sudo apt-get install nginx
```

2. 启动Nginx

```shell
sudo systemctl start nginx
```

3. 配置开机自启动Nginx

```shell
sudo systemctl enable nginx
```

4. 配置网站文件

Nginx 的默认配置文件存放在 `/etc/nginx` 目录下。你可以在该目录中的 `sites-available` 目录中创建一个新的配置文件，用于定义你的网站配置。

例如，为了启用AriaNg这个静态网站服务，你可以创建一个名为 `ariang` 的配置文件：

```shell
sudo nano /etc/nginx/sites-available/ariang
```

在文件中，你可以定义服务器块（server block）以及网站的根目录等设置，例如：

```conf
server {
    listen 80;
    server_name mydomain.com www.mydomain.com;

    root /var/www/ariang;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

5. **创建网站根目录**：

创建上述配置文件中指定的网站根目录：

```shell
sudo mkdir -p /var/www/ariang
```

将你的静态网站文件放置在 `/var/www/ariang` 目录中。

6. **通过软连接启用网站配置**：

创建一个指向 `sites-enabled` 目录中的符号链接，以启用你的网站配置：

```shell
sudo ln -s /etc/nginx/sites-available/ariang /etc/nginx/sites-enabled/
```

> 注意：启用新的内容前，需要删除site-enable下的默认软连接文件`default`

7. **测试配置**：

验证你的 Nginx 配置是否正确无误：

```shell
sudo nginx -t
```

8. **重启 Nginx**：

如果验证通过，重新加载 Nginx 配置：

```shell
sudo systemctl reload nginx
```

现在，你的树莓派上就配置好了 Nginx 服务器。

## 四、安装AiraNg

1. 安装AiraNg

```shell
cd /var/www/ariang
wget https://github.com/mayswind/AriaNg/releases/download/1.3.6/AriaNg-1.3.6.zip
unzip AriaNg-1.3.6.zip -d ariang
```

在浏览器中访问你的树莓派id地址即可打开AriaNg了

2. 连接AiraNg

这时AriaNg显示未连接，在“系统设置-AriaNg设置-全局-RPC（192.168.\*.\*）-Aria2 RPC 密钥 ”中，输入“secret” 即可连接！

之后，就可以愉快的用树莓派下载电影或者文件了～

> 这里的密钥secret可以在配置文件中修改；

## 五、安装MiniDLNA

1. 安装MiniDLNA

```shell
sudo apt-get install minidlna
```

2. **编辑配置文件**：

MiniDLNA 的配置文件位于 `/etc/minidlna.conf`。你可以使用文本编辑器来编辑这个文件，

```shell
sudo vim /etc/minidlna.conf
```

在配置文件中，你可以设置你想要共享的媒体文件夹、媒体库的名称等信息。确保配置文件中的设置与你的需求相匹配。

3. **启用 MiniDLNA 服务**：

在配置完成后，启用 MiniDLNA 服务并将其添加到启动时自动运行：

```shell
sudo systemctl enable minidlna
sudo systemctl start minidlna
```

4. **重启 MiniDLNA 服务**：

运行以下命令以重启 MiniDLNA 服务，使新的配置生效：

```shell
sudo systemctl restart minidlna
```

5. **配置文件写法**

   以下是一个简单的 `minidlna.conf` 配置文件示例，用于启用 MiniDLNA 服务器并共享指定的媒体文件夹。你可以根据自己的需求进行修改和调整。

```conf
# 此配置文件用于 MiniDLNA (ReadyMedia)

# 设置 MiniDLNA 使用的网络接口
network_interface=eth0

# 设置 MiniDLNA 共享的媒体文件夹路径
media_dir=/path/to/your/media/folder

# 设置 MiniDLNA 服务器名称
friendly_name=My MiniDLNA Server

# 设置 MiniDLNA 数据库缓存路径
db_dir=/var/cache/minidlna

# 设置 MiniDLNA 日志文件路径
log_dir=/var/log

# 设置 MiniDLNA 使用的端口
port=8200

# 设置 MiniDLNA 的通知频道
notify_interval=900

# 设置 MiniDLNA 是否允许图片预览
enable_thumbnail=yes

# 设置 MiniDLNA 是否允许视频截图
inotify=yes

# 设置 MiniDLNA 允许的媒体文件类型
media_types=AV

# 设置 MiniDLNA 使用的媒体格式
# 如果需要支持更多格式，你可以添加额外的格式类型
media_mime=audio/flac,audio/mp3,image/jpeg,image/png,video/mp4,video/x-matroska

# 设置 MiniDLNA 是否允许外部控制
enable_inotify=yes

```



## 六、安装Samba

推荐查看我的上一篇文章：树莓派安装Samba服务，实现家庭文件共享

## 七、推荐资源

> 电影天堂：https://www.dytt8.com/

## 八、参考引用

> https://ost.51cto.com/posts/1681