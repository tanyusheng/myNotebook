这是一个用Python开发的GUI实战项目：居民身份证信息校验系统
### 一、总体介绍
本项目使用Tkinter作为GUI模块，充分利用Python面向对象的思想，开发一款实现身份证号码校验的应用程序。具备解析用户输入的身份证号码中的地区信息、出生日期、以及身份证号码是否合法等功能。是练习Python面向对象思想、tkinter GUI模块的优质练手项目。
##### 项目演示
![4.gif](https://upload-images.jianshu.io/upload_images/5845585-e5cf85f4fca4a1c9.gif?imageMogr2/auto-orient/strip)
以上，如果我们输入一个正确的身份证号码，系统可以正常解析；但是篡改其中一位的话，校验结果直接显示无效；如果少输入一位的话，系统会提示“请输入18位”。
### 二、认识身份证号码
身份证号码的构成如下：
![屏幕快照 2020-08-12 上午12.40.47.png](https://upload-images.jianshu.io/upload_images/5845585-0cb200e4ac84b07e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
（1）地区码：身份证前6位就是地区码，中国每一个地区都对应一个地区码，按照GB/T2260执行。通常1开头为华北地区、2开头为东北地区、3开头为华东地区、4开头为华中地区和华南地区、5开头为西南地区、6开头为西北地区、7和8开头为特别地区。
（2）出生日期码：表示编码对象出生的年、月、日，按GB/T7408的规定执行，年月日代码之间不用分隔符。
（3）顺序码：表示在同一地址码所标识的区域范围内，对同年同月同日出生的人编订的顺序号，顺序码的奇数分配给男性、偶数分配给女性。
（4）校验码：身份证第18位是校验码，对前17位做一个运算，按照ISO 7064:1983.MOD 11-2校验码计算出来的检验码得到第18位的数字。
校验方法：
![屏幕快照 2020-08-16 下午2.50.41.png](https://upload-images.jianshu.io/upload_images/5845585-77737c71b3b25a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![屏幕快照 2020-08-12 上午12.59.42.png](https://upload-images.jianshu.io/upload_images/5845585-94e75ba0b91d8183.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 三、实现界面类：IDCheckGUI
在工程项目中新建一个idcheckgui.py的文件用来生成GUI界面
```
from tkinter import *
from tkinter.ttk import *
import os

class IDCheckGUI(Tk):
    def __init__(self):
        super().__init__()
        self.title("身份证信息校验系统")
        self.geometry("800x510+400+200")
        self.resizable(0,0)
        self["bg"] = "whitesmoke"
        self.setup_UI()

    def setup_UI(self):
        self.style01 = Style()
        self.style01.configure("input.TLabel",font=("微软雅黑",20,"bold"))
        self.style01.configure("TLabel",font=("微软雅黑",20,"bold"),foreground = "navy")
        self.style01.configure("TButton",font=("微软雅黑",20,"bold"),background = "lightblue")

        # 图片
        self.Login_image = PhotoImage(file = "."+os.sep+"img"+os.sep+"id2.png")
        self.Label_image = Label(self,image = self.Login_image)
        self.Label_image.place(x=5,y=5)
        # 输入信息
        self.Label_id_input = Label(self,text = "请输入身份证号码:",style="input.TLabel")
        self.Label_id_input.place(x=400,y=20)
        self.var_input = StringVar()
        self.Entry_id_input = Entry(self,textvariable = self.var_input,width=20,font=("微软雅黑",18,"bold"))
        self.Entry_id_input.place(x = 400,y=70)
        self.Button_id_input = Button(self,text="校验",command = self.get_info)
        self.Button_id_input.place(x = 660,y = 70)
        # 具体信息
        self.Label_is_exsit = Label(self,text = "是否有效：")
        self.Label_is_exsit.place(x=400,y=170)
        self.var_enable = StringVar()
        self.Entry_is_exsit = Entry(self, state=DISABLED,textvariable=self.var_enable, width=10, font=("微软雅黑", 18, "bold"))
        self.Entry_is_exsit.place(x=530, y=165)

        self.Label_is_gender = Label(self, text="性       别：")
        self.Label_is_gender.place(x=400, y=220)
        self.var_gender = StringVar()
        self.Entry_is_gender = Entry(self, state=DISABLED,textvariable=self.var_gender, width=10, font=("微软雅黑", 18, "bold"))
        self.Entry_is_gender.place(x=530, y=215)

        self.Label_is_birthday = Label(self, text="出生日期：")
        self.Label_is_birthday.place(x=400, y=270)
        self.var_birthday = StringVar()
        self.Entry_is_birthday = Entry(self, state=DISABLED,textvariable=self.var_birthday, width=18, font=("微软雅黑", 19, "bold"))
        self.Entry_is_birthday.place(x=530, y=265)

        self.Label_is_area = Label(self, text="所  在  地：")
        self.Label_is_area.place(x=400, y=320)
        self.var_area = StringVar()
        self.Entry_is_area = Entry(self,state=DISABLED, textvariable=self.var_area, width=18, font=("微软雅黑", 19, "bold"))
        self.Entry_is_area.place(x=530, y=315)

        self.Button_close = Button(self,text = "关闭",command = self.close_window)
        self.Button_close.place(x = 650, y= 450)

    def close_window(self):
        self.destroy()

    def get_info(self):
        self.var_enable.set("有效!")
```
由于我们使用面向对象的思想开发，我们把主函数放在另一个文件中`startcheck.py`中，在startcheck模块下导入我们实现GUI的模块`idcheckgui`
```
from idcheckgui import *
if __name__ == '__main__':
    check_gui = IDCheckGUI()
    check_gui.mainloop()
```
##### 运行演示
![1.gif](https://upload-images.jianshu.io/upload_images/5845585-88d3cbdbc1a0b392.gif?imageMogr2/auto-orient/strip)
现在我们只是搭建了GUI界面，并没有真正的进行校验操作。
> 注意：在使用面向对象思想导入自定义模块时，如果出现导入的包无法读取的情况。
> 方法一：最好在新建一个空工程的根目录下就放上所有的python程序文件；
> 方法二：或者鼠标选中工程目录，右键菜单选择`Mark Directory as`然后选择`Sources Root`即可。
### 四、实现功能类：IDCheck
##### 1. 检查校验码
(1) 对身份证号码进行切片
首先我们把获取到的身份证号码分成地区码、生日码、顺序码、校验码，四个部分，存储在列表`id_list[]`中。
通过`get_id_list`方法对身份证号码字符串进行切片：
```
def get_id_list(self):
    # 地区码
    self.id_list.append(self.id_number[:6])
    # 出生日期码
    self.id_list.append(self.id_number[6:14])
    # 顺序码
    self.id_list.append(self.id_number[14:17])
    # 校验码
    self.id_list.append(self.id_number[17:])
    return self.id_list
```
（2）根据前17位计算校验码
获取身份证号码的前17位存储在`number`中，然后对17位数字分别乘以系数`[7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]`，累加得出结果后对11进行取余，将获得的结果作为索引取出列表`["1","0","x","9","8","7","6","5","4","3","2"] `中的值即为校验码。
```
def get_check_number(self):
    """
    取出校验码
    :return: 返回的校验码
    """
    number = self.id_number[:17]
    xi_list = [7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2] # 每个位上乘的系数列表
    check_number = ["1","0","x","9","8","7","6","5","4","3","2"]    # 返回的校验码列表
    sum_of_number = 0
    for index in range(len(number)):
       sum_of_number += int(number[index]) * xi_list[index]
    # 余数
    yu_number = sum_of_number % 11
    return check_number[yu_number]
```
(3) 将计算出的校验码与身份证最后一位比较，我们提前在构造函数设置静态属性`self.is_true_id_number = 0`，如果校验码核对成功，便将其值设为1
```
def validate_check_number(self):
    if self.get_check_number() == self.id_list[3]:
        self.is_true_id_number = 1
```


##### 2. 检查出生日期
我们规定出生日期必须介于`1900-01-01`到当前的日期，只要时间在这个区间内就算有效，超过这个范围就算无效。
```
def validate_birthday(self):
    date_from = datetime(year=1900,month=1,day=1)
    date_to = datetime.today()
    id_birthday = datetime(year=int(self.id_number[6:10]),month=int(self.id_number[10:12]),day=int(self.id_number[12:14]))
    if id_birthday > date_from and id_birthday < date_to:
        self.birthday = self.id_number[6:10]+"年"+self.id_number[10:12]+"月"+self.id_number[12:14]+"日"
```
##### 3. 校验地区码
校验身份证号码中的地区码是否合法，我们主要需要完成两步操作：
(1)从文件导入地区码，存储在列表`area_list`中；
由于地区码与地区名的对应关系我们存储在一个`id_area.txt`的文件中
![屏幕快照 2020-08-15 下午9.31.25.png](https://upload-images.jianshu.io/upload_images/5845585-590b360c6c1d3eab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们可以通过读取文件中每一行的数据，使用逗号作为分隔符生成一个列表，再将该列表添加到area_list列表中。
```
def import_area_id(self):
    try:
        with open(file=self.file_path,mode="r",encoding="UTF-8") as fd:
            current_line = fd.readline()
            while current_line:
                current_area_list = current_line.split(",")
                if len(current_area_list[0]) == 6:
                    self.area_list.append(current_area_list)
                current_line = fd.readline()
    except:
        showinfo("系统提醒","地区文件读取失败")
```
（2）校验当前身份证上的地区码是否在列表中；
我们定义一个`validate_area_id`的方法，将从输入的身份证号码中的地区码与`area_list`中的地区码进行比对，从而获取对应的地区名
```
def validate_area_id(self):
    for index in range(len(self.area_list)):
        if self.area_list[index][0] == self.id_list[0]:
            self.area_name = self.area_list[index][1]
            break
```
##### 4. 识别身份证号码的性别
我们可以直接根据身份证号码的第三部分判断其奇偶数来确定性别，`id_list`列表的第三部分存储的是顺序码，将顺序码先转为整型然后对2取余。如果等于0说明是偶数，即女性；如果等于1说明是奇数，即男性。
```
def get_gender(self):
    if int(self.id_list[2]) % 2 == 0:
        self.gender = "女"
    else:
        self.gender = "男"
```
### 五、完成身份证的校验
我们在`id_checkgui`模块中,定义一个`get_info`函数用于对输入的身份证号码进行校验。
我们需要导入前面写的`idcheck`模块，使用该模块下的`IdCheck`类构造一个检验对象`check_id`,传入的参数为本模块GUI中输入框获取到的值。
校验逻辑为：
![屏幕快照 2020-08-16 下午1.11.05.png](https://upload-images.jianshu.io/upload_images/5845585-ac44d047f5cf64cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
def get_info(self):
    id_number = self.var_input.get()
    if len(id_number) == 18:
        check_id = idcheck.IdCheck(id_number)
        if check_id.is_true_id_number == 0 or len(check_id.birthday) == 0 or len(check_id.area_name) == 0:
            self.var_enable.set("无效!")
        else:
            self.var_enable.set("有效")
            self.var_gender.set(check_id.gender)
            self.var_birthday.set(check_id.birthday)
            self.var_area.set(check_id.area_name)
    else:
        self.var_enable.set("无效")
        self.var_gender.set("")
        self.var_birthday.set("")
        self.var_area.set("")
        showinfo("系统消息", "输入的身份证号码不满18位，请重新输入！")
```
最后再将GUI模块中的校验按钮添加`command`参数其值设置为`get_info`即可。
##### 效果演示：
情况一：输入的身份证号码不满18位(我们故意输入17位)：
![1.gif](https://upload-images.jianshu.io/upload_images/5845585-5ac1cb09a9a8049c.gif?imageMogr2/auto-orient/strip)
情况二：最后一位校验位错误（本来是7我们故意输入8）
![2.gif](https://upload-images.jianshu.io/upload_images/5845585-ab860305246b1af5.gif?imageMogr2/auto-orient/strip)
情况三：输入正确的身份证号码的情况
![3.gif](https://upload-images.jianshu.io/upload_images/5845585-026db4d8fcf9df7c.gif?imageMogr2/auto-orient/strip)
### 最后
本项目利用Tkinter开发了一个身份证号码校验系统，能够识别用户输入的身份证号码的有效性，并且解析身份证号码的地区、出生日期、性别等有效信息，感兴趣的小伙伴可以私信我获取全套的源码、素材、及数据源··