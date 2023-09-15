### 一、认识多继承
单继承：一个派生类的基类只有一个
多继承：一个派生类的基类有多个
一句话区分就是：单继承是一个生一个，多继承是多个生一个
多继承的基本语法：
```
class 类名(基类01，基类02，基类03)：
    pass
```
子类定义构造方法时，需要将父类的构造方法调用一次。
案例演示：
```
# 认识多继承
class A:
    def __init__(self):
        self.aa = "====A的属性====="
    def print_A(self):
        print("===A正在打印====")
class B:
    def print_B(self):
        print("====B正在打印====")

class C(A,B):   # 多继承
    def __init__(self):
        A.__init__(self)
        self.cc = "===C的属性==="
    def print_C(self):
        print("====C的方法=====")
    pass

if __name__ == '__main__':
    c = C()
    print(c.aa) # 继承了A类的属性
    c.print_A() # 继承了A类的方法
    c.print_B() # 继承了B类的方法
    print(c.cc) # 继承了C类的属性
    c.print_C() # 继承了C类的方法
```
### 二、多继承的初始化
在多继承中，所有基类的方法可以直接继承，但是属性需要手工初始化。如果派生类中没有`__init__`方法，则默认获得第一个类的属性。如果派生类中有`__init__`方法，则所有基类的属性都不会获得，需要手动逐一初始化。
```
class A:
    def __init__(self):
        self.aa = "====A的属性====="
    def print_A(self):
        print("===A正在打印====")
class B:
    def __init__(self):
        self.bb = "===B的属性===="
    def print_B(self):
        print("====B正在打印====")
# 实现多继承
class C(A,B):
    def __init__(self):
        A.__init__(self)    # 初始化A类
        B.__init__(self)    # 初始化B类
        self.cc = "===C的属性==="
    def print_C(self):
        print("===C正在打印====")

if __name__ == '__main__':
    c = C() # 实例化C类
    print(c.aa) # 调用A类的属性
    c.print_A() # 调用A类的方法
    c.print_B() # 调用B类的方法
    print(c.bb) # 调用B类是属性
    print(c.cc) # 调用C类的属性
```
### 三、多继承的使用演示
我们定义一个ChineseStudent类，其继承关系如下：
![屏幕快照 2020-09-27 下午3.15.17.png](https://upload-images.jianshu.io/upload_images/5845585-09777dd82d4b6104.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
调用基类的属性和方法
```
# Person
class Person:
    def __init__(self,name,gender,age):
        self.name = name
        self.gender = gender
        self.age = age
    def run(self):
        print("===走路===")

# Chinese
class Chinese(Person):
    def __init__(self,name,gender,age,china_id):
        super().__init__(name,gender,age)
        self.china_id = china_id
    def say(self):
        print("===你好===")

# Student
class Student:
    def __init__(self,sno,stu_class):
        self.sno = sno
        self.stu_class = stu_class
    def study(self):
        print("===我在学习===")
        
# ChineseStudent
class ChineseStudent(Chinese,Student):
    def __init__(self,name,gender,age,china_id,sno,stu_class):
        Chinese.__init__(self,name,gender,age,china_id) # 初始化Chinese
        Student.__init__(self,sno,stu_class)    # 初始化Student

if __name__ == '__main__':
    zhangsan = ChineseStudent("张三","男","24","342623199810234589","2111908923","计算机1班")
    print(zhangsan.name)    # 调用Person的属性
    zhangsan.run()  # 调用Person的方法
    print("身份证号码：",zhangsan.china_id)   # 调用Chinese属性
    zhangsan.say()  # 调用Chinese方法
    print(zhangsan.stu_class)   # 调用Student属性
    zhangsan.study()    # 调用Student方法
```
### 四、多继承的注意事项
在不同的基类中存在相同的方法，派生类对象调用方法时会调用哪个父类的方法呢？
默认情况下，如果多个基类中有相同的方法，越在前面，优先级越高。
为了避免后期没必要的错误，建议基类之间存在重名的属性和方法应该尽量避免。
### 五、多继承的MRO
MRO(Method Resolution Order)方法解析顺序
##### 1. 旧式类
如果是经典类（旧式类），MRO的方法--DFS（深度优先搜索）策略
![屏幕快照 2020-09-27 下午3.58.39.png](https://upload-images.jianshu.io/upload_images/5845585-68a89e009456f177.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> Python的经典类（旧式类）存在于Python2.x中，而Python3.x中已全部为新式类。在定义类时，写成class A：则为旧式类的写法，若写成class A(object)：则为新式类的写法
##### 2. 新式类
如果是新式类，MRO的方法--BFS（广度优先搜索）策略
![屏幕快照 2020-09-27 下午4.12.14.png](https://upload-images.jianshu.io/upload_images/5845585-1e56b325b76d6fb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)