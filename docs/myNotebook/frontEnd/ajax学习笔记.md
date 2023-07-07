# AJAX学习笔记

## 概念：

定义：AJAX是异步的JavaScript与XML，使用XMLHttpRequest对象与服务器通信；

## 一、axios的使用：

Axios是一个基于promise构建的一个网络请求库，可以用于浏览器和node.js

### 1.引入axios.js

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

### 2.使用axios函数

（1）传入配置对象

（2）使用`.then`回调函数接收结果，并做后续处理

```js
axios({
	url:'目标资源地址'
}).then(result=>{
	// 对服务器返回的数据做后续处理
})
```

### 3.案例:获取API中的省份列表，并在段落中显示出来；

```html
<!doctype html>
<html lang="zh-cmn-Hans">
<head>
    <meta charset="utf-8">
<link rel="icon" href="./img/favicon.ico">
    <title>axios使用</title>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body>
     <p class="my-p"></p>
</body>
<script>
    axios({
        url:'http://hmajax.itheima.net/api/province'
    }).then(result=>{
        // 对服务器返回的数据做后续处理
        console.log(result.data.list.join('<br>'));
        // 把准备好的省份列表插入到页面
        document.querySelector('.my-p').innerHTML = result.data.list.join('<br>');
    })
</script>
</html>
```

## 二、URL统一资源定位符



### 1.URL的概念

url是什么？URL是统一资源定位符，用于访问服务器上的资源；结构由协议、域名、资源路径组成；

### 2.URL查询参数

url参数查询语法：

```
http://xxx.com/xxx/xxx?参数名1=值1&参数名2=值2
```

### 3.axios查询参数

使用axios提供的params选项：

```js
axios({
    url:'目标资源地址',
    params:{
        参数名:值
    }
}).then(result=>{
    // 对服务器返回的数据做后续处理
})
```

实际上axios在运行时会把params中的内容自动拼接到url?参数名=值上面；

### 4.案例：地区查询

> 在ES6中如果属性名和变量同名，可以只写一个；

通过省份和城市名查询该城市的所有区县；

```js
document.querySelector('.sel-btn').addEventListener('click',() => {
  let pname = document.querySelector('.province').value;
  let cname = document.querySelector('.city').value;
  // 基于axios获取服务器数据
  axios({
    url: "http://hmajax.itheima.net/api/area",
    params: {
      pname,
      cname
    }
  }).then(result => {
    let list = result.data.list;
    // 利用map函数将list映射到新的列表中theLi中
    let theLi = list.map(areaName => `<li class="list-group-item">${areaName}</li>`).join('')
    console.log(theLi);
    //将结果插入到元素中显示出来
    document.querySelector('.list-group').innerHTML = theLi;
  })
})
```

### 5.常用的请求方法

| 请求方法 |   操作   |   备注   |
| :------: | :------: | :------: |
|   GET    | 获取数据 |          |
|   POST   | 提交数据 |          |
|   PUT    | 修改数据 | 全部数据 |
|  DELETE  | 删除数据 |          |
|  PATCH   | 修改数据 | 部分数据 |

axios配置请求方法怎么写？

GET方法是默认的可以省略，在参数列表中，如果是get方法则用params,如果是post方法则用data；

```js
axios({
    url:'目标资源地址',
    method:'请求方法',
    params:{
        参数名:值
    }
}).then(result=>{
    // 对服务器返回的数据做后续处理
})
```

### 6.axios错误处理

在axios请求错误时，通过调用`catch`方法传入回调函数并定义形参；例如在用户注册失败时，通过弹窗提示用户错误原因。

```js
axios({
  // 请求选项
}).then(result=>{
  // 对服务器返回的数据做后续处理
}).catch(error=>{
  // 处理错误
})
```

## 三、http协议

### 1.请求报文

Http协议：规定了浏览器发送以及服务器返回内容的格式；

请求报文：浏览器按照Http协议要求的格式，发送给服务器的内容

请求报文的组成部分有：

- 请求行：请求方法、URL、协议
- 请求头：以键值对的格式携带附加的信息
- 空行：分隔请求头，空行之后是发送给服务器的资源、
- 请求体：发送的资源

在浏览器中怎么查看请求报文：

在浏览器的浏览器开发者工具的网络面板中，选择Fetch/XHR中查看，

请求行和请求头在标头选项中，请求体在载荷选项中；

![屏幕快照 2023-06-17 16.50.54](localpicbed/ajax学习笔记.assets/屏幕快照 2023-06-17 16.50.54.png)

> 在故障排查时可以查看请求报文的请求体内容是否正确；

### 2.响应报文

响应报文的组成部分有:

- 响应行（状态行）：协议、HTTP响应状态码、状态信息
- 响应头：以键值对的格式携带附加信息，比如：Content-Type
- 空行：分隔响应头，空行之后是服务器返回的资源
- 响应体：返回的资源

HTTP响应状态码：用来表明请求是否成功，主要含义如下：

| HTTP状态码 | 说明       | 备注                |
| ---------- | ---------- | ------------------- |
| 1xx        | 信息       |                     |
| 2xx        | 成功       |                     |
| 3xx        | 重定向消息 |                     |
| 4xx        | 客户端错误 | 404服务器找不到资源 |
| 5xx        | 服务端错误 |                     |

## 四.接口文档

根据后端的接口文档，前端使用ajax进行调用；

示例API文档：https://apifox.com/apidoc/project-1937884/doc-1695440

### 1.案例-用户登录

- 点击登录时，判断用户名和密码长度；
- 提交数据和服务器通信
- 提示消息

```js
// 点击登录时，用户名和密码长度判断，并提交数据和服务器通信
// 定义一个函数用于显示提示框
function showAlert(msg, isSuccess) {
const alert = document.querySelector(".alert");
let alertStyle = isSuccess ? "alert-success" : "alert-danger";
alert.innerHTML = msg;
alert.classList.add("show");
alert.classList.add(alertStyle);
// 设置一个定时器3s后隐藏提示框
setTimeout(() => {
alert.classList.remove("show");
alert.classList.remove(alertStyle);
}, 3000);
}
// 登录点击事件
document
.querySelector(".btn-login")
.addEventListener("click", function () {
// 获取用户名和密码
const username = document.querySelector(".username").value;
const password = document.querySelector(".password").value;
// 对长度做判断
if (username.length < 8) {
console.log("用户名长度不能低于8");
showAlert("用户名长度不能低于8", false);
return;
}
if (password.length < 6) {
console.log("密码长度不能低于6");
showAlert("密码长度不能低于6", false);
return;
}
axios({
method: "post",
url: "http://hmajax.itheima.net/api/login",
data: {
    username,
    password,
},
})
.then((result) => {
    console.log(result.data.message);
    showAlert(result.data.message, true);
})
.catch((error) => {
    console.log(error.response.data.message);
    showAlert(error.response.data.message, false);
});
});
```

![屏幕快照 2023-06-17 23.36.03](localpicbed/ajax学习笔记.assets/屏幕快照 2023-06-17 23.36.03.png)

### 2.form-serialize插件

使用form-serialize插件快速收集表单元素的值

语法：

```js
const data = serialize(form, {hash: true, empty:true})
/**
 * 使用serialize函数，快速收集表单元素的值
 *  参数1：要获取的表单名称；
 *  参数2：配置对象 
 *       hash:设置获取数据结构
 *          - false:获取到的是url查询字符串
 *          - true:获取到的是JS对象
 *       empty:设置是否获取空值
 *          - false: 不获取空值
 *          - true: 可以获取到空值
*/
```

使用方法：

引入插件，调用serialize方法获取到表单元素的值，表单元素的`name`属性会成为返回对象的属性名；

将上面用户登录的案例利用`form-serialize`插件获取表单的值，下面是实际的写法：

```js
// 点击登录时，用户名和密码长度判断，并提交数据和服务器通信
// 定义一个函数用于显示提示框
function showAlert(msg, isSuccess) {
const alert = document.querySelector(".alert");
let alertStyle = isSuccess ? "alert-success" : "alert-danger";
alert.innerHTML = msg;
alert.classList.add("show");
alert.classList.add(alertStyle);
// 设置一个定时器3s后隐藏提示框
setTimeout(() => {
alert.classList.remove("show");
alert.classList.remove(alertStyle);
}, 3000);
}
// 登录点击事件
document
.querySelector(".btn-login")
.addEventListener("click", function () {
// 使用form-serialize插件
const form = document.querySelector(".form-example");
const data = serialize(form, { hash: true, empty: true });
// 使用解构赋值的方式获取对象中用户名和密码
const { username, password } = data;
// 对长度做判断
if (username.length < 8) {
console.log("用户名长度不能低于8");
showAlert("用户名长度不能低于8", false);
return;
}
if (password.length < 6) {
console.log("密码长度不能低于6");
showAlert("密码长度不能低于6", false);
return;
}
axios({
method: "post",
url: "http://hmajax.itheima.net/api/login",
data: {
    username,
    password,
},
})
.then((result) => {
    console.log(result.data.message);
    showAlert(result.data.message, true);
})
.catch((error) => {
    console.log(error.response.data.message);
    showAlert(error.response.data.message, false);
});
});
```

### 3.Bootstrap弹框

功能：不离开当前页面，显示单独内容，供用户操作；调用弹窗可以通过两种方式：属性控制和JS控制；

使用方法：

（1）引入bootstrap.css与bootstrap.js

（2）给启动弹框的组件添加bootstrap属性，显示弹框需要两个属性，分别是`data-bs-toggle`和`data-bs-target`

```html
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target=".my-box">显示弹框</button>
```

（3）通过自定义属性，控制弹框的显示和隐藏，隐藏弹框需要设置属性`data-bs-dismiss="modal"`

通过属性控制弹窗的显示和隐藏具体写法如下：

```html
<div class="container mt-3">
    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target=".my-box">显示弹框</button>
    <!-- 弹框标签 -->
    <div class="modal my-box" tabindex="-1">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">请输入姓名</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <span>姓名</span>
                    <input type="text" class="form-contorl" placeholder="默认姓名">
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        取消
                    </button>
                    <button type="button" class="btn btn-primary">
                        保存
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

![屏幕快照 2023-06-18 19.00.14](localpicbed/ajax学习笔记.assets/屏幕快照 2023-06-18 19.00.14.png)

通过JS控制，显示或隐藏弹框：

```js
// 创建弹框对象
const modalDom = document.querySelector('.my-box');
const modal = new bootstrap.Modal(modalDom);
// 显示弹框
modal.show();
// 隐藏弹框
modal.hide();
```



### 4.图书管理管理系统

获取图书列表的的API地址`http://hmajax.itheima.net/api/books`

#### （1）获取内容并渲染到网页上

>  这里重点需要掌握的技巧是使用map将列表对象中的元素映射到html元素中,并进行字符串拼接的过程。map函数可以传入三个参数(element、index、array)，具体用法可见：https://www.freecodecamp.org/chinese/news/javascript-map-how-to-use-the-js-map-function-array-method/

```js
window.addEventListener('load',()=>{
    const creator = '小雨';
    // 封装一个函数，获取并渲染图书列表
    function getBookList(){
        axios({
            url: 'http://hmajax.itheima.net/api/books',
            params: {
                creator,
            }
        }).then(result => {
            const bookList = result.data.data;
            console.log(bookList);
            const bookItemHtml = bookList.map((item,index) => {
                return `<tr>
                <td>${index + 1}</td>
                <td>${item.bookname}</td>
                <td>${item.author}</td>
                <td>${item.publisher}</td>
                <td>
                    <span class="del">删除</span>
                    <span class="edit">编辑</span>
                </td>
            </tr>`
            }).join('');
            document.querySelector('.list').innerHTML = bookItemHtml;
        })
    }
    // 执行函数
    getBookList()
})
```

![屏幕快照 2023-06-22 19.09.07](localpicbed/ajax学习笔记.assets/屏幕快照 2023-06-22 19.09.07.png)

#### （2）添加图书信息

```js
// 目标2：添加图书信息
    // 获取弹窗信息
const addModalDom = document.querySelector('.add-modal');
const addModal = new bootstrap.Modal(addModalDom);
document.querySelector('.add-btn').addEventListener('click',()=>{
    // 获取表单信息
    const bookItemForm = document.querySelector('.add-form');
    const bookItem = serialize(bookItemForm,{hash:true,empty:true});
    // 提交表单信息
    axios({
        url: 'http://hmajax.itheima.net/api/books',
        method: 'post',
        data:{
            // 这里的...是对象展开运算符
            ...bookItem,
            // 这里的creator是一个全局变量
            creator
        }
    }).then(result => {
        // 添加成功后请求重新渲染页面
        getBookList();
        // 重置表单，防止点击添加按钮后，上一次添加的内容还在
        bookItemForm.reset();
        // 隐藏弹框
        addModal.hide();
    })
});
```

#### （3）删除图书信息

> 思路：
>
> a.绑定点击事件获取图书id 
>
> b.调用删除接口
>
> c.刷新图书列表

由于每一行图书信息是动态生成的，如果给每一行图书的删除按钮添加点击事件，则需要委托其父级元素设置点击事件。再定义一个判断条件如果点击的元素中含有`del`类，则获取其父元素的`id`。

在渲染图书列表时就给删除按钮的父级元素创建一个自定义属性`data-id`，在点击事件中，查询父元素的`id`属性即可通过AJAX删除指定ID的元素列表；

在接口文档中看到需要提供PATH参数，这是一种新的传参方式（路径传参）

![屏幕快照 2023-07-02 00.48.14](localpicbed/ajax学习笔记.assets/屏幕快照 2023-07-02 00.48.14.png)

将URL改为模板字符串即可。

```js
// 目标3：删除图书信息
document.querySelector('.list').addEventListener('click',function(e) {
    if(e.target.classList.contains('del')){
        // 获取图书id
        const id = e.target.parentNode.dataset.id;
        console.log(id);
        // 调用删除接口
        axios({
            method: 'delete',
            url: `http://hmajax.itheima.net/api/books/${id}`,
        }).then(result =>{
            // 刷新图书列表
            getBookList();
        });
    }
})
```

#### （4）编辑图书信息

实现该功能主要需要完成以下部分的内容：

编辑弹窗的显示和隐藏；

获取当前图书的ID并通过查询接口获取图书信息填充到弹窗中；

点击提交按钮时，提交表单中的内容到后台，并重新渲染前端页面内容；

```js
// 目标4：编辑图书信息
// 获取编辑弹窗信息
const editModalDom = document.querySelector('.edit-modal');
const editModal = new bootstrap.Modal(editModalDom);
document.querySelector('.list').addEventListener('click',function(e){
    if(e.target.classList.contains('edit')){
        // 点击编辑按钮打开弹框
        editModal.show();
        // 获取图书id
        const theId = e.target.parentNode.dataset.id;
        // 获取图书详情信息
        axios({
            url: `http://hmajax.itheima.net/api/books/${theId}`,
        }).then(result => {
            const bookObj = result.data.data;
            const keys = Object.keys(bookObj);
            console.log(keys);
            keys.forEach(key => {
                document.querySelector(`.edit-form .${key}`).value = bookObj[key];
            });
        });
    }
})
// 点击修改按钮后保存数据提交到服务器
document.querySelector('.edit-btn').addEventListener('click',function(){
    // 获取编辑表单信息
    const editForm = document.querySelector('.edit-form');
    // 保存修改后的表单内容
    const {id,bookname,author,publisher} = serialize(editForm,{hash:true,empty:true});
    // 将表单中的内容提交到服务器
    axios({
            method: 'put',
            url: `http://hmajax.itheima.net/api/books/${id}`,
            data: {
                id,
                bookname,
                author,
                publisher,
                creator
            }
        }).then(() => {
            // 刷新页面
            getBookList();
            // 隐藏弹窗
            editModal.hide();
        });
})
```

### 5.图片上传

图片上传的思路：

（1）获取图片文件对象

给input标签添加一个change事件；

（2）使用FormData携带图片文件

```js
const fd = new FormData()
fd.append(参数名,值)
```

（3）提交表单数据到服务器

通过ajax将图片提交到服务器，并拿到后端返回的图片URL；

```js
document.querySelector('.upload').addEventListener('change',(e)=>{
    // 获取图片文件
    console.log(e.target.files[0]);
    // 使用FormData携带图片文件
    const fd = new FormData();
    fd.append('img',e.target.files[0])
    // 提交到服务器
    axios({
        url: 'http://hmajax.itheima.net/api/uploadimg',
        method: 'POST',
        data: fd
    }).then(result => {
        console.log(result.data.data);
        const imgurl = result.data.data.url;
        document.querySelector('.my-img').src = imgurl;
    })
})
```

