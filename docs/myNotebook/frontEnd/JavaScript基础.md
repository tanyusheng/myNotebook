### Javascript学习笔记

### 数值转换
1. 将字符串型转为数值型
* parseInt()
* parseFloat()
* Number()
* 使用 `-`、`*`、`/`
2. 将数值型转为字符串型
* toString(); 数值后调用toString方法；
* String(); String方法里面传参；
* \+ ' ';加上一个字符型变量；

3. 转为布尔类型，可以使用命令`Boolean()`这个命令；
   `0、NaN、null、undefined`会转成false，其余都会转为true
### 逻辑中断
逻辑与运算：
如果表达式1逻辑运算符是真，则返回表达式2运算符的值；如果表达式1为假，则返回表达式1的值；
```
conslole.log(123 && 456) 
返回的结果是456
```
逻辑或运算：
如果表达式1的值为假，则返回表达式2的值，否则返回表达式1的值；
### 数组
JavaScript数组里可以存放任意数据类型；
### 函数
在调用函数时，实参与形参的数量不一致，不会导致系统报错；如果实参多了，在多出来的实参不猜与；如果实参少了，则形参获取的值是`undefined`，如果此时参与数值运算的话，得到的结果是`NaN`
return关键字：
1. 在声明的函数体中，return如果返回多个用逗号隔开的值，则真正返回的是最后一个值；
2. 在函数声明时，return关键字后面的语句不会运行；
3. 如果函数没有return返回值，则返回的事`undefined`;
#### 函数声明的方式
第一种：命名函数
```
function fun(){
    console.log('hello');
}
```
第二种：匿名函数
```
var fun = function(){
    conslole.log('hello');
}
```
#### arguments
在JavaScript中，arguments是当前函数的一个`内置对象`，所有函数都内置了一个arguments对象，arguments对象中存储了传递的所有实参。
### 作用域
1. 全局作用域：整个script标签、或者是一个单独的js文件；
2. 局部作用域：在函数内部；
#### **特殊性质：**
在函数内部，如果将一个局部变量的修饰符去掉，该变量就可以当成全局变量来使用。前提是该函数需要被掉用。
```
    <script>
        function myFn(){
            var a= 0;
        }
        myFn();
        console.log(a);
    </script>

```
运行结果无法输出a的值，但是如果改成：
```
    <script>
        function myFn(){
            a= 0; // 去掉var
        }
        myFn(); // 这里必须要调用myFn()，否则依然无法读取a的值
        console.log(a);
    </script>

```
控制台就可以正常访问a的值；
### 预解析
1. JavaScript代码是有浏览器中的JavaScript解释器来执行的，解析器在运行JavaScript代码的时候分两步：`预解析`和`代码执行`。
* 预解析时js引擎会把js里面所有的`var`和`function`提升到当前作用域的最前面；
* 代码执行时是按照代码书写的顺序从上往下执行；
2. 预解析分为：变量预解析(变量提升)和函数预解析(函数提升);
提升时，只提升声明，不提升赋值。所以匿名函数一定要在调用前声明，否则无法执行
#### 案例：
```
var num = 10;
fun();
function fun(){
    console.log(num);
    var num = 20;
}
```
该代码的执行结果为：undefined
解题思路为：先进行最外层作用域的var变量和function函数进行预解析，解析完成后再对function函数内的作用域进行预解析；实际执行顺序为:
```
var num;
function fun(){
    var num;
    console.log(num);
    num = 20;
}
num = 10;
fun();
```
#### 案例：
```
 f1();
    console.log(c);
    console.log(b);
    console.log(a);
    function f1(){
        var a = b = c = 9;
        console.log(a);
        console.log(b);
        console.log(c);
    }
```
运行结果为：
```
9 9 9 9 9 报错
```
原因是：`var a=b=c=9;`相当于`var a = 9; b = 9; c = 9;`
预解析后相当于：
```
function f1(){
    var a;
    a = 9;
    b = 9;
    c = 9;
    console.log(a);
    console.log(b);
    console.log(c); 
}
f1();
console.log(c);
console.log(b);
console.log(a);
```
但如果写成`var a = 9, b = 9, c = 9;`则相当于
```
var a = 9;
var b = 9;
var c = 9;
```
那么运行结果则为：
```
9 9 9 报错
```
### 对象
JavaScript创建对象的三种方法：
* 利用字面量创建对象：`{}`
```
var obj = {}; // 创建了一个空的对象
var obj = {
    uname: 'xiaoyu',
    age: 18,
    gender: 'man',
    sayHi: function(){
        console.log('hello'); 
    }
}
```
多个属性和方法之间是用逗号隔开的，方法冒号后面跟的是一个匿名函数;
调用属性的方法可以通过对象名.属性调用，也可以通过对象名['属性名']的方式进行调用；
* 利用new Object创建对象
```
var obj = new Object();
    obj.name = 'xiaoyu';
    obj.age = '12';
    obj.sayHi = function(){
        console.log('hello');
    }
    //调用
    obj.sayHi();
    console.log(obj.name+obj.age);
```
* 利用构造函数创建对象
为了方便创建大量对象，构造函数就是将对象里相同的属性和方法抽象出来封装到函数里；
注意：构造函数首字母大写，属性需要使用this关键字；
```
function Star(uname,age,sex){
    this.name = uname;
    this.age = age;
    this.sex = sex;
    this.sing = function(song){
        console.log(song)
    }
}
var user = new Star('xiaoyu',18,man); // 创建一个对象
user.sing('take me to your heart');
```
#### new关键字执行时，系统中发生了什么
1. new 构造函数在内存中创建了一个空的对象； 
2. this就会指向刚才创建的空对象；
3. 执行构造函数里的代码，给这个空对象添加属性和方法；
4. 返回这个对象，这也就是为什么构造函数不需要return关键字的原因，因为new会返回对象。
#### 对象里的属性进行遍历，可以使用`for in`
```js
for(var k in obj){
    console.log(k); // 读取变量，读到的是属性名；
    console.log(obj[k]); //读取对象[变量]，读到的是属性值；
}
```
### 内置对象
JavaScript对象分为三种：自定义对象、内置对象、浏览器对象；
内置对象就是指JS语言自带的一些对象，供开发者使用，并提供一些常用的功能属性方法，常见的有Math、Date、Array、String等。
学会查询MDN文档。

1. 获得毫秒时间戳的三种方法：
```js
console.log(date.valueOf())
console.log(+new Date())
console.log(Date.now());
```
#### 时间对象：

定义时间戳对象，`new Date()`得到的是时间对象，可以使用getHours等方法；

 `+new Date()`获得的是number对象不能使用getHours等方法；

将字符串转为时间对象：

```js
var setTime = new Date('2023-5-1 12:00:00'); // 获取设置的时间
```

将时间对象转为时间戳：

```js
var setTimeStamp = Date.parse(setTime); // 获取设置的时间戳
```



2. 数组操作
```js
push('数组元素') // 从数组的后面添加元素，返回数组的长度
pop() // 从数组的后面删除元素，返回被删除的数组元素
unshift('数组元素') // 从数组的前面添加元素
shift() // 从数组的前面删除元素
```