### 抽象类的概念
抽象类是一个特殊的类，它只能被继承，不能被实例化。python中如果要使用抽象类则需要导入模块`abc`,
```
import abc
```
在定义抽象类的时候，需要添加参数`metaclass=abc.ABCMeta`,在定义抽象方法时需要在方法前添加声明
```
@abc.abstractmethod
```
注意，我们只需写出方法的名称，不用写出具体的方法内容。
例如：
```
# 抽象类和抽象方法
import abc

class Person(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def eat(self):
        pass
```
或者我们也可以写成
```
from abc import *

class Person(metaclass=ABCMeta):
    @abstractmethod
    def eat(self):
        pass
```
### 案例：使用抽象类和抽象方法来计算图形的周长和面积
```
from abc import *
import math

# 创建一个抽象类，计算周长和面积
class Graph(metaclass=ABCMeta):

    # 抽象方法求周长
    @abstractmethod
    def get_primeter(self): pass

    # 抽象方法求面积
    @abstractmethod
    def get_area(self): pass

# 派生出一个类 --- 等边三角形
class Equil_Triangle(Graph):
    def __init__(self, len):
        self.len = len

    # 实现抽象方法--求面积
    def get_area(self):
        return math.sqrt(3)/4*self.len*self.len

    # 实现抽象方法--求周长
    def get_primeter(self):
        return 3*self.len

if __name__ == '__main__':
    print("=== 等边三角形 ===")
    triangle01 = Equil_Triangle(10)
    print("这个三角形的边长是%s"%(triangle01.len))
    print("周长是%d 面积是%.2f"%(triangle01.get_primeter(), triangle01.get_area()))
```