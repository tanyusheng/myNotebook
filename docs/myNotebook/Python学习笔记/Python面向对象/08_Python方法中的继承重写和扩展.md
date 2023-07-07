### 一、继承
##### 1.封装与继承的概念
封装：根据需求将属性和方法封装到一个抽象的类中；
继承：获得父类的属性和方法，实现代码的复用
##### 2.继承的语法：
```
class 类名(父类名)
    具体的代码
```
案例：
```
class A:
    def __init__(self):
        self.name = "alice"
        self.age = "24"
    def show(self):
        print("====A=====")
        
class B(A):
    pass
```
##### 3. 常用术语
第一种：父类、子类、继承
A类是B类的父类，B类是A类的子类，B类由A类继承而来；
第二种：基类、派生类、派生
A类是B类的基类，B类是A类的派生类，B类由A类派生而来；

##### 4. 继承的案例
在前面GUI的开发过程中，实际上我们也可以使用继承的思想来开发。定义一个基类绘制基础的窗体框架，然后再定义若干派生类来派生基类，这样就能减少大量的重复工作。
```
from tkinter import *
from tkinter.ttk import *

# 创建GUI基类
class GUIBase:
    def __init__(self,title):
        self.frame = Tk()
        self.frame.title(title)
        self.frame.geometry("610x500+600+150")
        self.frame.resizable(0,0)
        self.frame["bg"] = "lightgray"
        # 配置Style
        self.frame.Style01 = Style()
        self.frame.Style01.configure("TPanedwindow",background = "whitesmoke")
        self.frame.Style01.configure("TButton",font=("微软雅黑",16,"bold"))
        self.frame.Style01.configure("TLabel", font=("微软雅黑", 20, "bold"))
        # 添加顶部的banner
        self.top_banner_image = PhotoImage(file = "./img/stu_detail_banner.png")
        self.frame.top_banner = Label(image = self.top_banner_image)
        self.frame.top_banner.place(x=5,y=5)
        # 添加一个Label标签
        self.Label01 = Label(self.frame,text = title)
        self.Label01.place(x=20,y=20)
        # 加载一个Pane容器
        self.frame.Pane_detail = PanedWindow(self.frame,width = 600,height = 370)
        self.frame.Pane_detail.place(x=5,y=95)
        # 添加最下面的关闭按钮
        self.frame.Button_close = Button(self.frame,text = "关闭",width = 10,command = self.close)
        self.frame.Button_close.place(x=470,y=470)

    def close(self):
        self.frame.destroy()

    def show(self):
        self.frame.mainloop()

# 派生出两个GUI派生类
class GUI01(GUIBase):
    def __init__(self,title):
        GUIBase.__init__(self,title)

        self.frame.Button_GUI01 = Button(self.frame,text = "GUI01",width = 10)
        self.frame.Button_GUI01.place(x=20,y=100)

class GUI02(GUIBase):
    def __init__(self,title):
        GUIBase.__init__(self,title)

        self.frame.Button_GUI02 = Button(self.frame,text = "GUI02",width = 10)
        self.frame.Button_GUI02.place(x=20,y=200)

if __name__ == '__main__':
    this = GUI02("添加学生信息")
    this.show()
```
我们定义了一个基类`GUIBase`和两个派生类`GUI01`和`GUI02`,在派生类中添加按钮，这样在通过派生类的构造方法构造对象时会自动具备基类的属性。
同样，基类的构造方法也是可以传递参数给派生类的，这里我们就定义了一个参数`title`这样我们就每次通过派生类实例化对象时传递参数`title`，基类的title变量就会自动获取值。

##### 5. 继承的传递性
子类会继承父类的属性和方法，并且具有传递性。父类的父类的方法也会传递下来，每一次继承的时候可以添加属性，子类继承当前类的时候会自动具备该属性。
```
# 父类
class Person:
    def __init__(self,name):
        self.name = name
    def walk(self):
        print("可以走路")

# 由Person类继承出Chinese类
class Chinese(Person):
    def __init__(self,name,id):
        Person.__init__(self,name)
        self.id = id
    def say(self):
        print("会说汉语")

# 由Chinese继承出ChineseStudent类
class ChineseStudent(Chinese):
    def __init__(self,name,id,sno):
        Chinese.__init__(self,name,id)
        self.sno = sno
    def study(self):
        print("准备高考")

if __name__ == '__main__':
    swift = ChineseStudent("张三","342612199810236832","3140705135")
    swift.walk()
    swift.say()
    print(swift.id)
    print(swift.sno)
```
我们定义一个父类Person类，具备属性：名字，具备方法：可以走路；定义一个子类Chinese继承Person类，添加属性:身份证号，添加方法：会说汉语；再定义一个子类ChineseStudent继承Chinese类，添加属性：学号，添加方法：学习；这样我们通过ChineseStudent类构造的对象具备Chinese的属性和方法，同样也具备Person类的属性和方法。
运行结果：
```
可以走路
会说汉语
342612199810236832
3140705134
```
##### 6. object类
Python中查看一个类的基类可以使用`类名.__base__`查看，常见的`int`、`list`其实也是一个类，我们可以查看其基类
```
# 查询一个类的基类
print(int.__base__)
print(list.__base__)
```
输出结果
```
<class 'object'>
<class 'object'>
```
我们发现它们的基类都指向了同一个类，那就是object类，object类是什么呢?
object类是Python中所有类的基类；
如何查看一个类中有哪些方法呢？
使用`dir(类名)`即可查看一个类中所具备的所有方法
```
print(dir(object))  # 查看object类中所具备的所有方法
```
运行结果：
```
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```
##### 7. 新式类和经典类
只有在Python2.x的版本中,才有这个特性，
旧式类创建方法：class Person:
新式类的创建方法：class Person(object)
在Python 3.x版本中，如果不指明父类，默认父类就是object
### 二、方法的重写和扩展
##### 1. 组合和继承
继承：自动获得父类所有的属性和方法
组合：需要手工拼接，访问的时候需要多层次的访问
```
class Person:
    def __init__(self,name):
        self.name = name
    def walk(self):
        print("走路")

class Chinese:
    def __init__(self,Person:Person,chinese_id):
        self.Person = Person
        self.chinese_id = chinese_id
    def speak_chinese(self):
        print("大家好，很高兴认识你！")

# ==== 使用 =======
if __name__ == '__main__':

    cool_boy = Person("张三")
    zhangsan = Chinese(cool_boy,"342612199710127863")
    # 使用两种方法访问Person01中的方法
    zhangsan.Person.walk()
    cool_boy.walk()
    # 使用两种方法访问Person01中的属性
    print(zhangsan.Person.name)
    print(cool_boy.name)
```
##### 2.方法的重写
在继承的过程中，如果继承来的方法不符合实际的引用，需要对方法进行重写。
重写规则：方法名称一样
某一个实例在访问某一个方法，父类中有这个方法，但父类的父类也有这个方法，然而实际上系统会调用父类中的方法。
```
# 基类
class Person:
    def __init__(self,name):
        self.name = name
    def eat(self):
        print("吃饭")

# 由Person类派生出Chinese类
class Chinese(Person):
    def __init__(self,name,id):
        Person.__init__(self,name)
        self.id = id

    def say(self):
        print("会说汉语")

    # 重写eat方法
    def eat(self):
        print("我是中国人,我爱吃米饭")

# 由Chinese派生出ChineseStudent类
class ChineseStudent(Chinese):
    def __init__(self,name,id,sno):
        Chinese.__init__(self,name,id)
        self.sno = sno
    def study(self):
        print("正在学习中国近代史")

if __name__ == '__main__':
    alice = ChineseStudent("李四","342356199012308789","3140989123")
    alice.eat()
```
运行结果：
```
我是中国人,我爱吃米饭
```
这里`Person`类定义了`eat()`方法，其子类`Chinese`重写了`eat()`方法。运行结果表明，当`Chinese`的子类`ChineseStudent`这个类实例化的对象`alice`调用`eat()`方法时，会以父类`Chinese`为准。
##### 3.方法的扩展
方法的重写使用的场景是：继承来的方法不符合我的实际应用场景；
方法的扩展使用的场景是：基类的方法符合我的应用，但是不完整，需要进行扩展。
扩展的原则：在子类中先重新方法，在重写的方法中使用`父类名.方法名`调用父类的方法，然后进行扩展。
```
# 父类
class Person:
    def __init__(self,name):
        self.name = name
    def eat(self):
        print("吃饭")

# 子类
class Chinese(Person):
    def __init__(self,name,id):
        Person.__init__(self,name)
        self.id = id

    # 扩展eat方法
    def eat(self):
        # 先执行父类中的eat方法
        Person.eat(self)
        # 再扩展自己的eat方法
        print("喜欢吃蛋炒饭")
if __name__ == '__main__':
    zhansan = Chinese("张三","342712199810126787")
    zhansan.eat()
```
运行结果
```
吃饭
喜欢吃蛋炒饭
```
案例中，子类扩展了父类中的eat方法，在扩展父类eat方法时，先通过`父类名.方法名`调用父类方法，添加自己的扩展方法。这样构造的实例在调用方法时会具备扩展的功能。
实际上还可以使用super关键字来进行方法的扩展,这时就不用使用self关键字了
```
# 父类
class Person:
    def __init__(self,name):
        self.name = name
    def eat(self):
        print("吃饭")

# 子类
class Chinese(Person):
    def __init__(self,name,id):
        super().__init__(name)
        self.id = id

    # 扩展eat方法
    def eat(self):
        # 使用super关键字扩展
        super().eat()
        # 扩展方法
        print("喜欢吃蛋炒饭")
if __name__ == '__main__':
    zhansan = Chinese("张三","342712199810126787")
    zhansan.eat()
```
super()就是一种指针指向父类，有了它调用父类方法，就不用再使用self关键字了。