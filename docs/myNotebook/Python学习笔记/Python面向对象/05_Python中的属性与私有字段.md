 ### 一、属性是什么
##### 1. 基本概念
字段：类变量和实例变量
属性：对普通方法修饰后实现特定的功能

在实例方法前添加装饰器`@property`,后面调用这个实例方法是可直接通过`对象.方法`来调用，可以省略括号；
```
class Calulator:
    pi = 3.1415926
    def __init__(self,radius):
        self.radius = radius

    # 求圆的周长
    @property
    def perimeter(self):
        return 2 * Calulator.pi * self.radius

    # 求圆的面积
    @property
    def area(self):
        return Calulator.pi * self.radius * self.radius

if __name__ == '__main__':
    this = Calulator(10)
    print("圆的周长",this.perimeter)
    print("圆的面积",this.area)
```
##### 2. 为什么需要属性
访问属性时可以制造出和访问字段完全相同的假象；
对字段值的保护
 ### 二、私有字段
在属性前加两个下划线，这个属性就不允许在类以外的地方访问
```
self.__gender = gender  # 实例变量 --- 私有字段 
```
私有字段只能在类的内部进行访问和调用，在类的外面无法访问。
```
class Person:

    __count = 0 # 类变量 ---- 字段
    def __init__(self,name,age,gender=""):
        self.name = name    # 实例变量  --- 公共字段
        self.age = age
        self.__gender = gender  # 实例变量 -- 私有字段

        # 每实例化一次，count+1
        Person.__count += 1

    @classmethod
    def get_count(cls):
        print("当前实例化的次数",str(cls.__count))

    def get_gender(self):
        print("我的性别为:",self.__gender)

if __name__ == '__main__':
    alice = Person("alice", "12", "女")
    bob = Person("bob", "22", "男")
    # 可以访问的字段：类、实例
    Person.get_count()
    print(alice.name)
    # 间接访问
    alice.get_gender()
```
把私有字段成员变量在类中打包成成员方法，就可以在类外间接访问；同理，私有字段类变量打包在类中打包成类方法，也可以在类外间接访问。

 ### 三、属性的基本使用
我们还是通过前面计算圆的周长和面积的类来举例：
```
class Calulator:
    pi = 3.1415926  # 类变量
    def __init__(self,radius):
        self.radius = radius    # 实例变量

    # 求圆的周长
    @property
    def perimeter(self):
        return 2 * Calulator.pi * self.radius

    # 求圆的面积
    @property
    def area(self):
        return Calulator.pi * self.radius * self.radius
```
pi的值是不允许修改的，我们如果没有对pi的值进行私有化，随便一个方法都可以改变pi的值。
对于客观存在的值，不允许外界修改的，我们应该将其设为私有变量。用户再调用的过程中是不可以修改的，但是可以写成接口，用户可以查看其值。
```
@classmethod
def get_pi(cls):
    return Calulator.__pi
```
除了pi,我把把半径设为私有字段，在构造函数中不再传入半径的值，通过成员方法设置半径，并且通过属性给私有字段半径赋值。
```
class Calulator:
    __pi = 3.1415926  # 类变量
    def __init__(self):
        self.__radius = 0    # 实例变量

    # 通过属性获取半径的值，构造函数中不用传入半径的值
    @property
    def radius(self):
        return self.__radius

    # 赋值给半径
    @radius.setter
    def radius(self,value):
        if not str(value).isdigit():
            raise ValueError("半径必须要符合要求：必须要是正整数")
        if value > 100:
            raise ValueError("半径的值必须要在0-100之间")
        else:
            self.__radius = value

    # 求圆的周长
    @property
    def perimeter(self):
        return 2 * Calulator.__pi * self.radius

    # 求圆的面积
    @property
    def area(self):
        return Calulator.__pi * self.radius * self.radius

    # 给用户查看pi值的接口
    @classmethod
    def get_pi(cls):
        return Calulator.__pi

if __name__ == '__main__':

    this = Calulator()
    # 获取半径值
    print(this.radius)
    # 赋值给半径
    this.radius = 10  # 这种属性赋值的方式很特殊！
    # 获取半径值
    print(this.radius)
    # 打印周长和面积
    print("周长",this.perimeter)
    print("面积",this.area)
```
> 注意：使用属性给私有字段赋值时：函数名要与使用了属性装饰器的函数名一致，并且装饰器要写成`函数名.setter`

这样做的好处：
（1）可以控制字段：设置可读、可写
（2）赋值或读取的时候，可以做有效性判断