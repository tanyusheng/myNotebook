Android常用的数据存储方式

1. SharePreference存储数据

2. 文件存储（内部、外部）
3. SQLite数据库存储
4. ContentProvider存储数据
5. 网络存储数据

## 本地文件操作

SharedPresferences

用于存放一些类似登录的配置信息

本质上是一个xml文件，是通过类似键值对的方式存放信息；

位于程序的私有目录中`data/data/[packageName]/shared_presfs                                                                                                                                                                                                            `

## Android数据库操作