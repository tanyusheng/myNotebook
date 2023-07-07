为了更加透彻的理解基础的概念，我们做一个综合案例
### 一、抽象的集合
我们打印一个填充数字的九宫格，然后分别让其顺时针旋转和逆时针旋转，该如何实现呢？
![屏幕快照 2020-08-27 下午3.06.49.png](https://upload-images.jianshu.io/upload_images/5845585-57eb8f2171431ebb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 1. 打印出九宫格
```
class Array:
    def __init__(self):
        self.list01 = [[1,2,3],[4,5,6],[7,8,9]]
    def print(self):
        # 打印集合
        for row in range(len(self.list01)):
            for col in range(len(self.list01[row])):
                print(self.list01[row][col],end="\t")
            # 换行
            print()
if __name__ == '__main__':
    this_array = Array()
    this_array.print()
```
然后将九宫格顺时针旋转，我们定义一个顺时针旋转的方法right_rotate；
```
def right_rotate(self):
    # 定义一个临时的集合,存储旋转后的结果
    temp_list = []
    # 右旋操作
    for row in range(len(self.list01)):
        temp_row = []
        for col in range(len(self.list01[row])-1,-1,-1):
            temp_row.append(self.list01[col][row])
        # 加载第一行到temp_list
        temp_list.append(temp_row)
    # 修改原先的集合
    self.list01 = temp_list
```
我们将原九宫格打印出来，再将顺时针旋转的九宫格打印出来
```
=========
1	2	3	
4	5	6	
7	8	9	
=========
7	4	1	
8	5	2	
9	6	3
```
由此可见上面写的将九宫格数字顺时针旋转的的方法是可行的，同样的道理我们再写出逆时针旋转的方法left_rotate，这里就不再赘述了~

### 二、Canvas绘图
Python Tkinter里有一个专门的模块canvas是用来绘图的，它可以绘制出线条、矩形、椭圆、三角形、文字等等。在使用这个模块的时候只需要导入tkinter包即可。这里我们创建一个Canvas的类，用来学习canvas的使用。
##### 1. 绘制画板
首先我们需要准备一个canvas画板，和常规的tkinter构建的设置模式一样，bg参数设置背景色，width和heigth设置长宽度。
```
from tkinter import *
cv = Canvas(self,bg = "whitesmoke",width = 700,height = 500)
cv.place(x=50, y=50)
```
这样我们就会绘制出一个具有画板的窗体
![屏幕快照 2020-08-30 下午5.04.29.png](https://upload-images.jianshu.io/upload_images/5845585-929273e31e41d0eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 2. 绘制线条
画一根线（给出开头和结尾的坐标，fill为填充色，dash为虚线的实虚像素长度，(5,5)即表示画出一段5像素的实线再画出一段5像素的虚线
```
cv.create_line(50,15,600,15,fill = "red")
cv.create_line(50,20,600,20,fill = "green",dash = (5,5))
```
![屏幕快照 2020-08-30 下午5.08.16.png](https://upload-images.jianshu.io/upload_images/5845585-815ebb92a41c3181.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 3. 绘制矩形
画一个矩形形（给出左上角和右下角顶点的的坐标），fill参数为填充的颜色，outline参数为边框颜色，width参数为边框的厚度
```
cv.create_rectangle(60,40,100,80,fill = "white",outline = "gray",width = 5)
cv.create_rectangle(120,40,200,80,fill = "gray",outline = "green",width =5)
```
![屏幕快照 2020-08-30 下午5.09.42.png](https://upload-images.jianshu.io/upload_images/5845585-dca39bb159b7d9ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 4. 绘制椭圆
画椭圆(相当于绘制一个矩形，在矩形中内切一个圆形或者椭圆)
```
cv.create_oval(60,100,100,140,fill = "red")
cv.create_oval(120,100,200,140,fill = "green")
```
![屏幕快照 2020-08-30 下午5.16.08.png](https://upload-images.jianshu.io/upload_images/5845585-8cdf3d168d97384a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 5. 绘制三角形
画三角形（指定三个点的坐标）
```
cv.create_polygon(280,40,240,100,300,100,fill = "blue")
```
![屏幕快照 2020-08-30 下午5.26.33.png](https://upload-images.jianshu.io/upload_images/5845585-bde030f8c93f7f80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 6. 绘制文字
绘制文字时给出坐标，text介绍文字，fill填充颜色
```
cv.create_text((320,50),text = "小雨",fill = "red")
```
![屏幕快照 2020-08-30 下午5.30.13.png](https://upload-images.jianshu.io/upload_images/5845585-ca9a3e268d4d99b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 三、把集合抽象成图形
在前面知识的基础上我们将九宫格二维数组的数值转化为可显示的颜色块，`[[1,1,0],[0,1,0],[0,1,0]]`就可以显示出相应的九宫格图像块。
```
class Tetris(Array):
    def __init__(self):
        super().__init__()
        # 修改list
        self.list01 = [[1,1,0],[0,1,0],[0,1,0]]
        # 绘制图形
        self.setup_UI()
    def setup_UI(self):
        self.frame = Tk()
        self.frame.title("俄罗斯方块")
        self.frame.geometry("340x640+400+50")
        # 准备画布
        self.cv = Canvas(self.frame,bg = "whitesmoke",width = 300,height = 600)
        self.cv.place(x=20,y=20)
        # 展示图形
        self.show_pic()
    def show_pic(self):
        """
        在画布中画出相应的图形
        """
        for row in range(len(self.list01)):
            for col in range(len(self.list01[row])):
                if self.list01[row][col] == 1:
                    # 打印30*30的方块
                    self.cv.create_rectangle(90+30*col,90+30*row,90+30*col+30,90+30*row+30,fill="navy",outline="whitesmoke",width=3)

    def show(self):
        self.frame.mainloop()
```
![屏幕快照 2020-08-30 下午6.06.29.png](https://upload-images.jianshu.io/upload_images/5845585-33f937af4ebd9881.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 四、实现图像的旋转
在前面Array类中就定义了旋转的方法right_rotate和left_rotate方法，这里的Tetris继承了Array类，所以可以直接调用旋转方法。这里我们还希望添加功能：
* 按下“L”表示逆时针旋转图形；
* 按下“R”表示顺时针旋转图形；
这里我们需要响应键盘的事件，通过获取响应事件的keycode码来判断哪个键被按下了。这里可查阅键盘[keycode对照表](http://www.phpweblog.net/kiyone/archive/2007/04/19/1138.html)。
L --- 76
R --- 82
在Tetris中定义响应键盘事件的方法：
```
self.frame.bind("<KeyPress>",self.rotate)
```
再定义一个我们绑定的rotate方法，我们先通过提示框来测试键盘监听是否成功。
```
def rotate(self,event):
    if event.keycode == 76:
        showinfo("系统消息","你按了L键")
    elif event.keycode == 82:
        showinfo("系统消息","你按了R键")
```
> 这里识别键盘按键内容除了使用keycode标识，也可以使用keysym标识，按键直接对应相应字符。L --- “L”、R --- “R”
> 当我们按下按键，系统提示框成功弹出后我们再进行真正的旋转操作。
```
def rotate(self,event):
    if event.keysym in ["L","l"]:
        # 逆时针旋转
        self.left_rotate()
        # 重新打印
        self.show_pic()

    elif event.keysym in ["R","r"]:
        # 顺时针旋转
        self.right_rotate()
        # 重新打印
        self.show_pic()
```
但是在实际打印的过程中，旧的图像不能及时擦除，我们需要在show_pic方法中将二维数组中字符值为0的块设置颜色与背景色相同
```
def show_pic(self):
    """
    在画布中画出相应的图形
    """
    for row in range(len(self.list01)):
        for col in range(len(self.list01[row])):
            if self.list01[row][col] == 1:
                # 打印30*30的方块
                self.cv.create_rectangle(90+30*col,90+30*row,90+30*col+30,90+30*row+30,fill="navy",outline="whitesmoke",width=3)
            else:
                # 用于擦除不满足条件的图形
                self.cv.create_rectangle(90+30*col,90+30*row,90+30*col+30,90+30*row+30,fill="whitesmoke",outline="whitesmoke",width=3)
```
实际运行的结果：
![1.gif](https://upload-images.jianshu.io/upload_images/5845585-1b848ece8edba357.gif?imageMogr2/auto-orient/strip)