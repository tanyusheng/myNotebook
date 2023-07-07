### 一、类的组合和创建
在一个类里再嵌套多个类，叫做类的组合，组合类可以更加有条理的组合类的各种信息。
我们可以将每一个小类的构造的对象作为参数传入大类中，这样就可以通过使用`大类构造的对象.大类的属性.小类的属性`,来访问每一个具体的属性。
我们以一台电脑为例，大类就是指计算机类其属性包含了小类：基本信息、硬件信息、操作系统信息、用户信息，而这些小类又包含了一系列具体的信息。如果我们想访问具体的属性，就需要通过组合类来实现；
```
# 基本信息
class Basic_info:
    def __init__(self,brand,type,color,weight,price,asset_number,buy_time):
        self.brand = brand
        self.type = type
        self.color = color
        self.weight = weight
        self.price = price
        self.asset_num = asset_number
        self.buy_time = buy_time
# 硬件
class Hardware:
    def __init__(self,memory,cpu,disk,monitor):
        self.memory = memory
        self.cpu = cpu
        self.disk = disk
        self.monitor = monitor
# 操作系统
class OS:
    def __init__(self,company,name,key):
        self.company = company
        self.name = name
        self.key = key
# 软件
class SoftWare:
    def __init__(self,*args):
        self.soft_list = args
# 用户信息
class UserInfo:
    def __init__(self,user_id,use_name,dept,position):
        self.user_id = user_id
        self.user_name = use_name
        self.dept = dept
        self.position = position
# 计算机类
class Computer:
    def __init__(self,basic_info,hardware,os,software,user_info):
        self.basic_info = basic_info
        self.hardware = hardware
        self.os = os
        self.software = software
        self.user_info = user_info

# 实例化
if __name__ == '__main__':
    # 实例化明细 --- 基本信息、硬件信息、操作系统、软件信息、用户信息
    alice_basic = Basic_info("Apple","MacBook Pro","macOS","1.368","12898","2111905163","2015-11-01")
    alice_hardware = Hardware("8","i5-5287","512","13.3")
    alice_os = OS("Apple","macOS","1111-2222-3333")
    alice_software = SoftWare("keynote","office","cad","wps")
    alice_user_info = UserInfo("22011","swift","开发部","主管")

    # 实例化电脑 --- 把明细结合起来
    alice_pc = Computer(alice_basic,alice_hardware,alice_os,alice_software,alice_user_info)

    # 获取某个属性
    print(alice_pc.basic_info.brand)
    print(alice_pc.basic_info.type)
```
这样就打印输出电脑基础信息中的品牌和型号。

### 二、组合类的输出
computer_detail.py
```
# 基本信息
class Basic_info:
    def __init__(self,brand,type,color,weight,price,asset_number,buy_time):
        self.brand = brand
        self.type = type
        self.color = color
        self.weight = weight
        self.price = price
        self.asset_num = asset_number
        self.buy_time = buy_time

    def show(self):
        print("==============基本信息==============")
        print("品牌:%s"%self.brand)
        print("型号:%s" % self.type)
        print("颜色:%s" % self.color)
        print("重量:%s" % self.weight)
        print("价格:%s" % self.price)
        print("资产编号:%s" % self.asset_num)
        print("购买时间:%s" % self.buy_time)

# 硬件
class Hardware:
    def __init__(self,memory,cpu,disk,monitor):
        self.memory = memory
        self.cpu = cpu
        self.disk = disk
        self.monitor = monitor
    def show(self):
        print("==============硬件信息==============")
        print("内存:%s GB"%self.memory)
        print("CPU:%s" % self.cpu)
        print("磁盘:%s GB" % self.disk)
        print("显示器:%s寸" % self.monitor)

# 操作系统
class OS:
    def __init__(self,company,name,key):
        self.company = company
        self.name = name
        self.key = key
    def show(self):
        print("==============操作系统==============")
        print("操作系统厂商:%s"%self.company)
        print("操作系统名称:%s" % self.name)
        print("操作系统序列号:%s" % self.key)

# 软件
class SoftWare:
    def __init__(self,*args):
        self.soft_list = args
    def show(self):
        print("==============软件信息==============")
        print("安装的软件有:")
        for item in self.soft_list:
            print(" ♦ "+item)
# 用户信息
class UserInfo:
    def __init__(self,user_id,user_name,dept,position):
        self.user_id = user_id
        self.user_name = user_name
        self.dept = dept
        self.position = position
    def show(self):
        print("==============用户信息==============")
        print("员工编号:%s"%self.user_id)
        print("员工姓名:%s" % self.user_name)
        print("员工部门:%s" % self.dept)
        print("员工职位:%s" % self.position)
```
computer.py
```
from computer_detail import *

# 计算机类
class Computer:
    def __init__(self,basic_info,hardware,os,software,user_info):
        self.basic_info = basic_info
        self.hardware = hardware
        self.os = os
        self.software = software
        self.user_info = user_info

    def show(self):
        print("###### 计算机的信息如下：######")
        self.basic_info.show()
        self.hardware.show()
        self.os.show()
        self.software.show()
        self.user_info.show()
        print("############################")


# 实例化
if __name__ == '__main__':
    # 实例化明细 --- 基本信息、硬件信息、操作系统、软件信息、用户信息
    alice_basic = Basic_info("Apple","MacBook Pro","macOS","1.368","12898","2111905163","2015-11-01")
    alice_hardware = Hardware("8","i5-5287","512","13.3")
    alice_os = OS("Apple","macOS","1111-2222-3333")
    alice_software = SoftWare("keynote","office","cad","wps")
    alice_user_info = UserInfo("22011","swift","开发部","主管")

    # 实例化电脑 --- 把明细结合起来
    alice_pc = Computer(alice_basic,alice_hardware,alice_os,alice_software,alice_user_info)
     # 打印
    alice_pc.show()
```