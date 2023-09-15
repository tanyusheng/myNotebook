### 一、什么是私有属性
Python中一个类主要由静态特征和动态特征构成。
静态特征的标准称呼为：字段（实例、类）
动态特征的标准称呼为：方法（实例方法、类方法、静态方法）
对于某些字段，如果在其它类中可以对其直接访问和修改，会对其程序带来很大的隐患。为了保护某些字段不能随便更改，我们将其设置为私有字段，只有在类中才可以访问。
定义私有字段时，在变量名前加上两个下划线`__变量名`。
为了读取私有字段我们可以定义字段同名方法，并添加装饰器`@property`，这样在外部类中可以像类的普通字段一样读取。
为了修改私有字段我们可以定义字段的同名方法，传入参数为待修改的值。装饰器为`@方法名.setter`
```
class Person:
    def __init__(self,name,age):
        self.name = name
        self.__age = age
    def say_hello(self):
        print("我的名字叫%s,今年%s岁了"%(self.name,self.__age))
    # 读取属性
    @property
    def age(self):
        return self.__age
    # 修改属性
    @age.setter
    def age(self,number):
        self.__age = number
if __name__ == '__main__':
    alice = Person("xiaoyu","23")
    alice.say_hello()
    alice.age = 12
    print(alice.age)
```
### 二、私有属性能继承吗
私有属性是不能继承的，只能在类的内部调用。子类是无法调用父类的私有属性的。·