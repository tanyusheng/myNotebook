### 一、需求分析
##### 1. 需要实现的功能
（1）通过控制台输入绿灯、黄灯、红灯的时间
（2）输入完成后，按回车，先绿灯倒计时，然后黄灯倒计时，然后红灯倒计时，再到绿灯倒计时，周而复始。
##### 2. 对类的分析
##### 静态特征
（1）三个数字：红灯、黄灯、绿灯
（2）两个电子屏
*  一个电子屏显示一个数字
* 一个电子屏包含200个（20行*10列），控制200个灯的亮与不亮显示数字
* 每个灯就两种状态:(亮与不亮)
##### 动态特征
* 打印
* 把数字构造在电子屏中
* 用不同的颜色打印
* 倒计时
##### 3. 如何实现时间的倒计时
每次打印完成后--> 停止1秒 --> 清屏 --> 在数字减一后再打印 --> 停止1秒后 --> 清屏

### 二、构建类的静态特征
##### 1. 构建一个Light类
创建一个显示屏含有20行10列的小灯，每个小灯设置为布尔型值False；
```
class Light:
    # 构造函数
    def __init__(self):
        self.light = []
        # 自动初始化
        self.prepare_light()

    def prepare_light(self):
        # 准备20行10列200个灯
        for row in range(20):
            temp = [] # 每行10个
            for col in range(10):
                # 每一列的灯默认不亮
                temp.append(False)
            # 把每行的10个插入到light集合中
            self.light.append(temp)
```

##### 2. 再构建一个`TrafficLight`类
接收红黄绿三种颜色灯要显示的时间：
```
class TrafficLight:
    # 构造函数
    def __init__(self,green_time,yellow_time,red_time):
        self.green_time = green_time    # 绿灯的时间
        self.yellow_time = yellow_time  # 黄灯的时间
        self.red_time = red_time    # 红灯的时间

        self.number01 = Light() # 显示第一个数字的电子屏
        self.number02 = Light() # 显示第二个数字的电子屏
```
### 三、实现红绿灯输入时间的校验
在前面我们给红绿灯运行时间直接提供了数字，按照实际需求我们应当手动输入红绿灯运行的时间。
我们定义一个方法可以根据提供的颜色，输入指定颜色灯光的时间。把这个方法定义成静态方法就可以不用通过对象来调用。
之后我们再检验输入的值是否符合要求：
* 输入的内容必须是数字
* 输入的数字范围在1-99之间

如果不符合要求，则提醒用户重新输入
```
@staticmethod           # 静态方法，不需要实例化直接调用！！！
def input_time(color:str):
    """
    根据提供的颜色，输入相应颜色的时间
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    :param color: 提供的颜色
    :return: 输入的时间
    """
    while True:
        time = ""    # 定义一个全局的time
        # 根据颜色提醒输入
        if color.lower() == "green":
            time = input("请输入绿灯的时间:")
        if color.lower() == "yellow":
            time = input("请输入黄灯的时间:")
        if color.lower() == "red":
            time = input("请输入红灯的时间:")
        # 校验输入的是否符合要求：数字、正数、1-99
        if not time.isdigit():
            print("输入的值不符合要求！【要求：1-99之间的数字】")
            continue    # 结束当前循环
        else:
            time_number = int(time)
            if time_number < 1 or time_number > 99:
                print("输入的值不符合要求！【要求：1-99之间的数字】")
                continue
            else:
                # 符合要求
                return time_number
```
### 三、实现红绿灯的倒计时功能
在控制台显示红绿灯效果的流程：清屏 --> 打印数字 --> 停止1秒 --> 打印减一后的数字
```
def start(self):
    while True:
        # 默认一直循环下去
        # 绿灯
        for number in range(self.green_time,-1,-1):
            os.system("clear")  # 清屏(windows中换为cls)
            print("%02d"%number)    # 这里的2表示占用两个宽度，如果不够宽度，前面补零
            time.sleep(1)   # 停止1秒钟

        # 黄灯
        for number in range(self.yellow_time,-1,-1):
            os.system("clear")
            print("%02d"%number)
            time.sleep(1)

        # 红灯
        for number in range(self.red_time,-1,-1):
            os.system("clear")
            print("%02d"%number)
            time.sleep(1)
```
### 四、实现不同的颜色输出
为了让控制台实现不同的颜色输出，我们可以使用第三方库`colorama`,它支持window、macOS、Linux平台，设置控制台输出的文字颜色。
##### 1. 安装colorama
```
pip install colorama
```
导入colorama模块
```
from colorama import init,Fore,Back,Style  
init(autoreset=True)    # 初始化Colorama
```
对于要指定输出颜色的字符串只需要在字符串前加上`Fore.RED`、`Fore.GREEN`、`Fore.YELLOW`即可输出红绿黄的颜色。
### 五、构建电子屏上的数字
我们定义一个方法`build_LED_number`接收一个数字字符，来构建一个显示数字的矩阵，可以通过两次for循环来实现，这里以绘制数字6为例，其余的数字的绘制方法也以此类推;
```
elif char == "6":
    for row in range(20):
        for col in range(10):
            if row < 2 or row > 17:
                temp_LED.light[row][col] = True
            if row == 9 or row == 10:
                temp_LED.light[row][col] = True
            if col < 2 :
                temp_LED.light[row][col] = True
            if col >7 and row > 10 :
                temp_LED.light[row][col] = True
```
### 六、实现数字的显示屏打印
我们怎么把倒计时的数字与要打印的数字做关联呢？
在开始做红绿灯倒计时的函数`start`调用显示函数`start_display`,传入逐个减小的数字`number`,以及相应的参数`color`;
```
def start(self):
    """
    开始红绿灯的倒计时
    """
    while True:
        # 默认一直循环下去
        # 绿灯
        for number in range(self.green_time,-1,-1):
            # 流程：清屏 --> 打印数字 --> 停止1秒 --> 清屏 --> 打印减一后的数字
            os.system("clear")  # 清屏(windows中换为cls)
            self.start_display(number,"green")  # 调用函数开始用特定的颜色打印
            # print(Fore.GREEN + "%02d" % number)    # 这里的2表示占用两个宽度，如果不够宽度，前面补零
            time.sleep(1)   # 停止1秒钟

        # 黄灯
        for number in range(self.yellow_time,-1,-1):
            os.system("clear")
            self.start_display(number, "yellow")
            # print(Fore.YELLOW +"%02d" % number)
            time.sleep(1)

        # 红灯
        for number in range(self.red_time,-1,-1):
            os.system("clear")
            self.start_display(number, "red")
            # print(Fore.RED + "%02d" % number)
            time.sleep(1)
```
> 注意这里的清屏函数，在Mac系统下使用`clear`，在windows系统下使用参数`cls`

我们的设置显示函数`start_display`接收两个参数`number`和`color`,先把接收到的整型数值`number`格式化成字符串，第一个字符通过`bulid_LED_number`构建成number01，第一个构建成number02,然后通过函数`print_LED`再电子屏上显示出来:
```
def print_LED(self,color:str):
    for row in range(20):
        # 打印第一个数字
        for col01 in range(10):
            if self.number01.light[row][col01] == True:
                if color == "green":
                    print(Fore.GREEN + " ▉ ",end="")
                elif color == "yellow":
                    print(Fore.YELLOW + " ▉ ", end="")
                elif color == "red":
                    print(Fore.RED + " ▉ ", end="")
            else:
                print(" □ ",end="")
        print("\t",end="")  # 两个数字之间的空格
        # 打印第二个数字
        for col02 in range(10):
            if self.number02.light[row][col02] == True:
                if color == "green":
                    print(Fore.GREEN + " ▉ ", end="")
                elif color == "yellow":
                    print(Fore.YELLOW + " ▉ ", end="")
                elif color == "red":
                    print(Fore.RED + " ▉ ", end="")
            else:
                print(" □ ", end="")
        # 换行
        print()
```
### 七、效果演示

![1.gif](https://upload-images.jianshu.io/upload_images/5845585-1c5ad0d95c2d16d3.gif?imageMogr2/auto-orient/strip)
建议在系统终端中运行，pycharm或者其他编辑器由于控制台空间有限，无法流畅输出效果。
### 附录
全部源码
```

import time # 让程序停止
import os
from colorama import init,Fore,Back,Style  # 导入颜色模块

init(autoreset=True)    # 初始化Colorama

class Light:
    # 构造函数
    def __init__(self):
        self.light = []
        # 自动初始化
        self.prepare_light()

    def prepare_light(self):
        # 准备20行10列200个灯
        for row in range(20):
            temp = [] # 每行10个
            for col in range(10):
                # 每一列的灯默认不亮
                temp.append(False)
            # 把每行的10个插入到light集合中
            self.light.append(temp)

class TrafficLight:
    # 构造函数
    def __init__(self,green_time,yellow_time,red_time):
        self.green_time = green_time    # 绿灯的时间
        self.yellow_time = yellow_time  # 黄灯的时间
        self.red_time = red_time    # 红灯的时间

        self.number01 = Light() # 显示第一个数字的电子屏
        self.number02 = Light() # 显示第二个数字的电子屏

    def bulid_LED_number(self,char:str):
        """
        根据提供的数字来构建电子屏上的数字显示
        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        :param char:提供的数字
        :return:返回构建的电子屏
        """
        temp_LED = Light()  # 新建一个电子屏屏幕，在修改这个电子屏
        if char == "0":
            for row in range(20):
                for col in range(10):
                    if row < 2: # 最上面两行
                        temp_LED.light[row][col] = True
                    if row > 17: # 最下面两行
                        temp_LED.light[row][col] = True
                    if col < 2: # 左边两列
                        temp_LED.light[row][col] = True
                    if col > 7: # 最右边两列
                        temp_LED.light[row][col] = True
        elif char == "1":
            for row in range(20):
                for col in range(10):
                    if col > 7:  # 最右边两列
                        temp_LED.light[row][col] = True
        elif char == "2":
            for row in range(20):
                for col in range(10):
                    if row < 2:  # 最上面两行
                        temp_LED.light[row][col] = True
                    if row <9 and col >7:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if row > 10 and col < 2:
                        temp_LED.light[row][col] = True
                    if row > 17:
                        temp_LED.light[row][col] = True
        elif char == "3":
            for row in range(20):
                for col in range(10):
                    if row < 2 or row > 17:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col > 7:
                        temp_LED.light[row][col] = True
        elif char == "4":
            for row in range(20):
                for col in range(10):
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col < 2 and row < 9:
                        temp_LED.light[row][col] = True
                    if col > 7 :
                        temp_LED.light[row][col] = True
        elif char == "5":
            for row in range(20):
                for col in range(10):
                    if row < 2 or row > 17:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col < 2 and row < 9:
                        temp_LED.light[row][col] = True
                    if col > 7 and row > 10:
                        temp_LED.light[row][col] = True
        elif char == "6":
            for row in range(20):
                for col in range(10):
                    if row < 2 or row > 17:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col < 2 :
                        temp_LED.light[row][col] = True
                    if col >7 and row > 10 :
                        temp_LED.light[row][col] = True

        elif char == "7":
            for row in range(20):
                for col in range(10):
                    if row < 2 :
                        temp_LED.light[row][col] = True
                    if col > 7:
                        temp_LED.light[row][col] = True
        elif char == "8":
            for row in range(20):
                for col in range(10):
                    if row < 2 or row > 17:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col < 2 or col > 7:
                        temp_LED.light[row][col] = True
        elif char == "9":
            for row in range(20):
                for col in range(10):
                    if row < 2 or row > 17:
                        temp_LED.light[row][col] = True
                    if row < 9 and col < 2:
                        temp_LED.light[row][col] = True
                    if row == 9 or row == 10:
                        temp_LED.light[row][col] = True
                    if col > 7:
                        temp_LED.light[row][col] = True
        # 返回这个LED
        return temp_LED

    def print_LED(self,color:str):
        for row in range(20):
            # 打印第一个数字
            for col01 in range(10):
                if self.number01.light[row][col01] == True:
                    if color == "green":
                        print(Fore.GREEN + " ▉ ",end="")
                    elif color == "yellow":
                        print(Fore.YELLOW + " ▉ ", end="")
                    elif color == "red":
                        print(Fore.RED + " ▉ ", end="")
                else:
                    print(" □ ",end="")
            print("\t",end="")  # 两个数字之间的空格
            # 打印第二个数字
            for col02 in range(10):
                if self.number02.light[row][col02] == True:
                    if color == "green":
                        print(Fore.GREEN + " ▉ ", end="")
                    elif color == "yellow":
                        print(Fore.YELLOW + " ▉ ", end="")
                    elif color == "red":
                        print(Fore.RED + " ▉ ", end="")
                else:
                    print(" □ ", end="")
            # 换行
            print()

    def start_display(self,number:int,color:str):
        """
        把传递过来的数字用指定的颜色打印
        :param number: 指定的数字
        :param color: 指定的颜色
        :return: 无返回值
        """
        # 把数字格式化
        number_str = "%02d" % number
        # 构建LED上显示的两个数字
        self.number01 = self.bulid_LED_number(number_str[0])
        self.number02 = self.bulid_LED_number(number_str[1])
        # 在电子屏上展示
        self.print_LED(color)

    def start(self):
        """
        开始红绿灯的倒计时
        """
        while True:
            # 默认一直循环下去
            # 绿灯
            for number in range(self.green_time,-1,-1):
                # 流程：清屏 --> 打印数字 --> 停止1秒 --> 清屏 --> 打印减一后的数字
                os.system("clear")  # 清屏(windows中换为cls)
                self.start_display(number,"green")  # 调用函数开始用特定的颜色打印
                # print(Fore.GREEN + "%02d" % number)    # 这里的2表示占用两个宽度，如果不够宽度，前面补零
                time.sleep(1)   # 停止1秒钟

            # 黄灯
            for number in range(self.yellow_time,-1,-1):
                os.system("clear")
                self.start_display(number, "yellow")
                # print(Fore.YELLOW +"%02d" % number)
                time.sleep(1)

            # 红灯
            for number in range(self.red_time,-1,-1):
                os.system("clear")
                self.start_display(number, "red")
                # print(Fore.RED + "%02d" % number)
                time.sleep(1)

    @staticmethod           # 静态方法，不需要实例化直接调用！！！
    def input_time(color:str):
        """
        根据提供的颜色，输入相应颜色的时间
        ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        :param color: 提供的颜色
        :return: 输入的时间
        """
        while True:
            time = ""    # 定义一个全局的time
            # 根据颜色提醒输入
            if color.lower() == "green":
                time = input(Fore.GREEN + "请输入绿灯的时间:")
            if color.lower() == "yellow":
                time = input(Fore.YELLOW + "请输入黄灯的时间:")
            if color.lower() == "red":
                time = input(Fore.RED + "请输入红灯的时间:")
            # 校验输入的是否符合要求：数字、正数、1-99
            # 如果不符合要求怎么处理：1. 出现异常，系统退出，报错；2.提醒重新输入
            if not time.isdigit():
                print("输入的值不符合要求！【要求：1-99之间的数字】")
                continue    # 结束当前循环
            else:
                time_number = int(time)
                if time_number < 1 or time_number > 99:
                    print("输入的值不符合要求！【要求：1-99之间的数字】")
                    continue
                else:
                    # 符合要求
                    return time_number

if __name__ == '__main__':
    # 输入红绿黄灯的时间

    green_time = TrafficLight.input_time("green")
    yellow_time = TrafficLight.input_time("yellow")
    red_time = TrafficLight.input_time("red")
    # 实例化
    traffic01 = TrafficLight(green_time, yellow_time, red_time)
    # 开始倒计时
    traffic01.start()
```