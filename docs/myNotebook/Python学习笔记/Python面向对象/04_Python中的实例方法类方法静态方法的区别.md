### 一、方法总体介绍
方法：实现某一个功能的程序块，在类里面以`def`开头的，是类中的基本组成成分。
类方法加关键字
```
@classmethod
```
静态方法加关键字
```
@staticmethod
```
我们写一个类，在里面分别定义：实例方法、类方法、静态方法：
```
class Person:

    person_number = 0
    def __init__(self,name,gender): # 构造方法：用来给对象初始化
        self.name = name
        self.gender = gender

    def say_hello(self):    # 实例方法 --- 应用在具体的对象上
        print("大家好，我叫"+self.name+",我是"+self.gender+"生！")
        Person.person_number += 1

    @classmethod
    def print_person_number(cls): # 类方法 --- 应用在类上
        print("当前有："+str(cls.person_number)+"人！")

    @staticmethod
    def print_welcome(person_name:str): # 静态方法 --- 应用在类上
        print("欢迎您"+person_name)

if __name__ == '__main__':
    peter = Person("peter","男")
    # 调用实例方法
    peter.say_hello()

    # 调用类方法
    Person.print_person_number()

    # 调用静态方法
    Person.print_welcome("Alice")
```

### 二、实例方法
##### 1. 知识储备
（1）实例方法实际占用的是类空间，实例化一个对象后，对象会绑定这个实例方法；
（2）使用`对象名.方法名`调用实例方法；
（3）self是一个指向对象的指针，使用self关键字调用当前对象的实例变量。
##### 2. 实例方法的案例
为了巩固实例方法的知识，我们做一个综合案例：使用Tkinter搭建一个提交学生信息的窗体。
提交学生信息前应满足的条件：
* 学号：95开头的6位数
* 姓名：汉字（2-10）
* 性别：只能填写男或者女
* 手机号码：11位数字
* 邮箱地址：满足基本格式
如果都满足条件，提交的时候跳出提示框“添加成功”
如果有一个不满足条件，跳出提示框并提示具体的哪一个不满足条件。
```
from tkinter import *
from tkinter.ttk import *
from tkinter.messagebox import *
import re

class Add_Student(Tk):
    def __init__(self):
        super().__init__()
        self.title("添加学生")
        self.resizable(0,0)
        self.geometry("420x350+350+150")
        self["bg"] = "whitesmoke"
        self.setup_UI()

    def setup_UI(self):
        self.style01 = Style()
        self.style01.configure("TLabel",font=("微软雅黑",20,"bold"))
        self.style01.configure("TButton", font=("微软雅黑", 20, "bold"))

        self.Label_sno = Label(self,text="学号:")
        self.Label_sno.place(x=80,y=20)
        self.var_sno = StringVar()
        self.Entry_sno = Entry(self,textvariable = self.var_sno,width=10,font=("微软雅黑",20,"bold"))
        self.Entry_sno.place(x=150,y=20)

        self.Label_name = Label(self, text="姓名:")
        self.Label_name.place(x=80, y=70)
        self.var_name = StringVar()
        self.Entry_name = Entry(self,textvariable = self.var_name,width=10, font=("微软雅黑", 20, "bold"))
        self.Entry_name.place(x=150,y=70)

        self.Label_gender = Label(self, text="性别:")
        self.Label_gender.place(x=80, y=120)
        self.var_gender = StringVar()
        self.Entry_gender = Entry(self,textvariable = self.var_gender, width=10, font=("微软雅黑", 20, "bold"))
        self.Entry_gender.place(x=150, y=120)

        self.Label_mobile = Label(self, text="手机:")
        self.Label_mobile.place(x=80, y=170)
        self.var_mobile = StringVar()
        self.Entry_mobile = Entry(self,textvariable = self.var_mobile,width=15, font=("微软雅黑", 20, "bold"))
        self.Entry_mobile.place(x=150, y=170)

        self.Label_email = Label(self, text="邮箱:")
        self.Label_email.place(x=80, y=220)
        self.var_email = StringVar()
        self.Entry_email = Entry(self,textvariable = self.var_email, width=15, font=("微软雅黑", 20, "bold"))
        self.Entry_email.place(x=150, y=220)

        self.Button_submit = Button(self,text = "提交",command = self.check_input)
        self.Button_submit.place(x = 280,y=290)

    def check_input(self):
        # 获取输入的值
        sno = self.var_sno.get()
        name = self.var_name.get()
        gender = self.var_gender.get()
        mobile = self.var_mobile.get()
        email = self.var_email.get()

        # 实例化
        current = Student(sno,name,gender,mobile,email)

        # 校验输入
        check_result = current.check_all()
        if check_result == 1:
            showinfo("系统消息","学号不符合要求,【要求：95开头的6位数字】")
        elif check_result == 2:
            showinfo("系统消息","姓名不符合要求，【要求：2-10个汉字】")
        elif check_result == 3:
            showinfo("系统消息","性别不符合要求，【要求：只能填男或者女】")
        elif check_result == 4:
            showinfo("系统消息","手机号码不符合要求，【要求：手机号码规范】")
        elif check_result == 5:
            showinfo("系统消息","邮箱地址不符合要求，【要求：邮箱规范要求】")
        elif check_result == 0:
            showinfo("系统消息", "添加成功！")

class Student():
    def __init__(self,sno:str,name:str,gender:str,mobile:str,email:str):
        self.sno = sno
        self.name = name
        self.gender = gender
        self.mobile = mobile
        self.email = email

    def check_sno(self):    # 校验学号
        pattern = re.compile(R"^95\d{4}$")
        match_result = pattern.match(self.sno)
        if match_result is None:
            return False
        else:
            return True

    def check_name(self):   # 校验姓名
        if len(self.name.strip())>=2 and len(self.name.strip())<=10:
            # 校验每一个字符是否都是汉字
            for index in range(len(self.name.strip())):
                if self.name[index] <= "\u4E00" or self.name[index] >= "\u9FA5":
                    return False
                # 校验到最后一个汉字仍然在编码范围内
                if index == len(self.name.strip())-1:
                    return True
        else:
            return False

    def check_gender(self): # 校验性别
        if self.gender.strip() in ["男","女"]:
            return True
        else:
            return False

    def check_mobile(self): # 校验手机号码
        pattern = re.compile(R"^[1][35789]\d{9}$")
        match_result = pattern.match(self.mobile)
        if match_result is None:
            return False
        else:
            return True

    def check_email(self):  # 校验邮箱
        pattern = re.compile(R"\w{1,}[@]\w{1,}[.]\w{1,}")
        match_result = pattern.match(self.email)
        if match_result is None:
            return False
        else:
            return True

    def  check_all(self):   # 总体校验
        if not self.check_sno():    # 学号不符合
            return 1
        elif not self.check_name(): # 姓名不符合
            return 2
        elif not self.check_gender():   # 性别不符合
            return 3
        elif not self.check_mobile():   # 手机号码不符合
            return 4
        elif not self.check_email():    # 邮箱不符合
            return 5
        else:
            return 0    # 全部符合条件

if __name__ == '__main__':

    add_gui = Add_Student()
    add_gui.mainloop()
```
##### 演示效果：
![1.gif](https://upload-images.jianshu.io/upload_images/5845585-0a6d804d68724adb.gif?imageMogr2/auto-orient/strip)
### 三、类方法
##### 1. 类方法的介绍
（1）使用`@classmethod`关键字开头
（2）类方法存储在类空间，没有绑定到对象上
（3）使用`类名.方法名`访问
（4）类方法可以访问类变量，但不能访问实例变量 ；
（5）在定义类方法的时候通常使用`cls`指针，调用类变量通过`cls.类变量`来访问
##### 2. 类方法案例演示
和前面一样，我们实现一个添加学生信息的窗体，不过这次我们使用类方法来实现。
我们创建一个类（含类方法的类）
```
class Student01():
    sno = ""    # 类变量
    name = ""
    gender = ""
    mobile = ""
    email = ""

    def __init__(self,sno,name,gender,mobile,email):
        Student01.sno = sno
        Student01.name  = name
        Student01.gender = gender
        Student01.mobile = mobile
        Student01.email = email
    @classmethod
    def check_sno(cls):    # 校验学号
        pattern = re.compile(R"^95\d{4}$")
        match_result = pattern.match(cls.sno.strip())
        if match_result is None:
            return False
        else:
            return True

    @classmethod
    def check_name(cls):   # 校验姓名
        if len(cls.name.strip())>=2 and len(cls.name.strip())<=10:
            # 校验每一个字符是否都是汉字
            for index in range(len(cls.name.strip())):
                if cls.name[index] <= "\u4E00" or cls.name[index] >= "\u9FA5":
                    return False
                # 校验到最后一个汉字仍然在编码范围内
                if index == len(cls.name.strip())-1:
                    return True
        else:
            return False

    @classmethod
    def check_gender(cls): # 校验性别
        if cls.gender.strip() in ["男","女"]:
            return True
        else:
            return False

    @classmethod
    def check_mobile(cls): # 校验手机号码
        pattern = re.compile(R"^[1][35789]\d{9}$")
        match_result = pattern.match(cls.mobile)
        if match_result is None:
            return False
        else:
            return True

    @classmethod
    def check_email(cls):  # 校验邮箱
        pattern = re.compile(R"\w{1,}[@]\w{1,}[.]\w{1,}")
        match_result = pattern.match(cls.email)
        if match_result is None:
            return False
        else:
            return True

    @classmethod
    def  check_all(cls):   # 总体校验
        if not cls.check_sno():    # 学号不符合
            return 1
        elif not cls.check_name(): # 姓名不符合
            return 2
        elif not cls.check_gender():   # 性别不符合
            return 3
        elif not cls.check_mobile():   # 手机号码不符合
            return 4
        elif not cls.check_email():    # 邮箱不符合
            return 5
        else:
            return 0    # 全部符合条件
```
注意：与实例方法的类不同的是，类方法的方法名前加上关键字`@classmethod`,并且使用`cls`指针，指向当前这个类。
定义好了使用类方法的类后，在GUI类中调用时，通过`类名.方法名`调用.
### 四、静态方法
##### 1. 静态方法的介绍
（1）使用`@staticmethod`关键字开头；
（2）静态方法存储在类空间，既没有绑定到成员变量也没有绑定到类；
（3）使用`类名.方法名`访问
（4）静态方法不能访问到类中的类变量和实例变量，(self、cls这两个关键字都不可使用）
（5）使用静态方法不需要实例化，就相当于是一个工具，拿来即用。
##### 2. 静态方法案例
同样我们还是以前面搭建添加学生信息窗体为案例，使用静态方法来实现：
```
from tkinter import *
from tkinter.ttk import *
from tkinter.messagebox import *
import re

class Add_Student(Tk):
    def __init__(self):
        super().__init__()
        self.title("添加学生")
        self.resizable(0,0)
        self.geometry("420x350+350+150")
        self["bg"] = "whitesmoke"
        self.setup_UI()

    def setup_UI(self):
        self.style01 = Style()
        self.style01.configure("TLabel",font=("微软雅黑",20,"bold"))
        self.style01.configure("TButton", font=("微软雅黑", 20, "bold"))

        self.Label_sno = Label(self,text="学号:")
        self.Label_sno.place(x=80,y=20)
        self.var_sno = StringVar()
        self.Entry_sno = Entry(self,textvariable = self.var_sno,width=10,font=("微软雅黑",20,"bold"))
        self.Entry_sno.place(x=150,y=20)

        self.Label_name = Label(self, text="姓名:")
        self.Label_name.place(x=80, y=70)
        self.var_name = StringVar()
        self.Entry_name = Entry(self,textvariable = self.var_name,width=10, font=("微软雅黑", 20, "bold"))
        self.Entry_name.place(x=150,y=70)

        self.Label_gender = Label(self, text="性别:")
        self.Label_gender.place(x=80, y=120)
        self.var_gender = StringVar()
        self.Entry_gender = Entry(self,textvariable = self.var_gender, width=10, font=("微软雅黑", 20, "bold"))
        self.Entry_gender.place(x=150, y=120)

        self.Label_mobile = Label(self, text="手机:")
        self.Label_mobile.place(x=80, y=170)
        self.var_mobile = StringVar()
        self.Entry_mobile = Entry(self,textvariable = self.var_mobile,width=15, font=("微软雅黑", 20, "bold"))
        self.Entry_mobile.place(x=150, y=170)

        self.Label_email = Label(self, text="邮箱:")
        self.Label_email.place(x=80, y=220)
        self.var_email = StringVar()
        self.Entry_email = Entry(self,textvariable = self.var_email, width=15, font=("微软雅黑", 20, "bold"))
        self.Entry_email.place(x=150, y=220)

        self.Button_submit = Button(self,text = "提交",command = self.check_input)
        self.Button_submit.place(x = 280,y=290)

    def check_input(self):
        # 获取输入的值
        sno = self.var_sno.get()
        name = self.var_name.get()
        gender = self.var_gender.get()
        mobile = self.var_mobile.get()
        email = self.var_email.get()

        if not Student02.check_sno(sno):
            showinfo("系统消息", "学号不符合要求,【要求：95开头的6位数字】")
        elif not Student02.check_name(name):
            showinfo("系统消息", "姓名不符合要求，【要求：2-10个汉字】")
        elif not Student02.check_gender(gender):
            showinfo("系统消息", "性别不符合要求，【要求：只能填男或者女】")
        elif not Student02.check_gender(mobile):
            showinfo("系统消息","手机号码不符合要求，【要求：手机号码规范】")
        elif not Student02.check_email(email):
            showinfo("系统消息", "邮箱地址不符合要求，【要求：邮箱规范要求】")
        else:
            showinfo("系统消息", "添加成功！")
            
# 使用静态方法
class Student02():
    @staticmethod
    def check_sno(sno):    # 校验学号
        pattern = re.compile(R"^95\d{4}$")
        match_result = pattern.match(sno.strip())
        if match_result is None:
            return False
        else:
            return True

    @staticmethod
    def check_name(name):   # 校验姓名
        if len(name.strip())>=2 and len(name.strip())<=10:
            # 校验每一个字符是否都是汉字
            for index in range(len(name.strip())):
                if name[index] <= "\u4E00" or name[index] >= "\u9FA5":
                    return False
                # 校验到最后一个汉字仍然在编码范围内
                if index == len(name.strip())-1:
                    return True
        else:
            return False

    @staticmethod
    def check_gender(gender): # 校验性别
        if gender.strip() in ["男","女"]:
            return True
        else:
            return False

    @staticmethod
    def check_mobile(mobile): # 校验手机号码
        pattern = re.compile(R"^[1][35789]\d{9}$")
        match_result = pattern.match(mobile)
        if match_result is None:
            return False
        else:
            return True

    @staticmethod
    def check_email(email):  # 校验邮箱
        pattern = re.compile(R"\w{1,}[@]\w{1,}[.]\w{1,}")
        match_result = pattern.match(email)
        if match_result is None:
            return False
        else:
            return True

if __name__ == '__main__':

    add_gui = Add_Student()
    add_gui.mainloop()
```
使用静态方法时就不用`self`、`cls`指针关键字了，在类中定义什么方法就接收什么参数。在GUI中调用时，也不需要实例化，直接通过`类名.方法名`来调用即可。
### 五、总结
以上，我们对于一个具体案例，通过三种不同的方法实现出来了。
很多时候同样一个需求，使用实例方法、类方法、静态方法都可以实现。三种方法都要熟练的掌握，并应用在实际的开发过程中。