### 前言

VScode-Git-HTML-CSS-JavaScript-jQuery-Bootstrap-Ajax-Vue.js-React-AngularJS

---
从今天开始，我们学习前端的知识，从最简单的HTML开始入手；HTML全称(Hyper Text Markup Language，超文本标记语言)，我们所需要学习的是各种标签🏷，通过这些标签来搭建网页的`骨架
`。
本篇文章主要由两个部分构成，从了解各个浏览器内核，然后重点学习HTML的各种常用标签，让我们开始学习吧~~
---
### 一、了解浏览器内核
##### 1. 什么是浏览器内核
浏览器内核。 浏览器中最重要或者说最核心的部分，也可以称为是“Rendering Engine”，可大概译为“解释引擎”，不过我们一般习惯将之称为“浏览器内核”。负责对网页语法的解释（如HTML、Java Script）并渲染（显示）网页。通常所谓的浏览器内核也就是浏览器所采用的渲染引擎，渲染引擎决定了浏览器如何显示网页的内容以及页面的格式信息。

##### 2. 五大主流浏览器以及四大内核

![web](localpicbed/HTML学习笔记(上).assets/web.png)

| 浏览器  |      内核      |                     备注                     |
| :-----: | :------------: | :------------------------------------------: |
|   IE    |    Trident     |    国内很多双核浏览器其中一核就是Trident     |
| Firefox |     Gecko      |                 代码完全公开                 |
| Safari  |     webkit     | Android默认浏览器、Symbian浏览器用都是webkit |
| Chrome  | Chromium/Blink |      大部分国内浏览器用的都是blink内核       |
|  Opera  |     Blink      |       13年以前Opera使用的是Presto内核        |


### HTML语法规范
成对出现的标签`<html> </html>`
单个出现的标签`<br \>`

##### 基本结构标签
基本骨架标签:

```
<html>
	<head>
		<meta charset="utf-8">
		<title>这是一个标题 <\title>
	</head>
	<body>
		这是正文
	</body>
</html>
```
##### HTML常用标签
标题标签`<h1><h6>`

段落标签`<p>这是一个段落</p>`段落之间有间隔

换行标签`<br />`


##### 文本格式化标签
加粗 `<strong> </strong>`或者`<b> </b>`

倾斜`<em> </em>`或者`<i> </i>`

删除线`<del> </del>`或者`<s> </s>`

下划线`<ins> </ins>`或者`<u> </u>`

##### 盒子标签
分割标签`<div> </div>`

div标签用来布局，一行只能放一个div，它可以看成是一个大盒子

跨度标签`<span> </span>`

span标签也可以用来布局，一行能放多个span,它可以看成是一个小盒子。

##### 图像标签
图像标签`<img src="图像URL">`

图像标签的属性：

|  属性  |  属性值  |                   说明                   |
| :----: | :------: | :--------------------------------------: |
|  src   | 图片路径 |                 必须属性                 |
|  alt   |   文本   |  替换文本，当图像不能显示时，显示的文字  |
| title  |   文本   |    提示文本，鼠标放到图像上显示的文字    |
| width  |   像素   |              设置图像的宽度              |
| height |   像素   | 设置图像的高度，一般设置一个即可自动缩放 |
| border |   像素   |     设置图像边框粗细,一般在css中设定     |

**注意事项**

属性采用键值对的格式，属性之间不分顺序用空格隔开。

**相对路径**

* 访问同级路径，可以直接使用文件名
* 访问下一级文件，使用`/`
* 访问上一级文件，使用`../`

##### 超链接标签

超链接的语法格式
`<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>`

|  属性  |                             作用                             |
| :----: | :----------------------------------------------------------: |
|  href  | 必要属性，指定为URL,`#`可以表示为空链接,如果为文件路径，可以作为下载链接使用。 |
| target | 用于指定链接页面打开方式，`_self`为默认值，`_blank`为新窗口打开 |

href的值设置为`#`依然会在URL后添加一个`#`,真正阻止连接跳转，href的值可以设置为`javascript:;`或者`javascript:void(0);`

##### 锚点链接

锚点连额吉可以快速定位到页面中的某个位置

定义目标位置标签，添加id属性，值设为自定义名称；
在超链接href属性中，设置属性值为`#前面定义的id名称`

##### 注释标签和特殊字符
`<!--注释内容-->`可以使用`command+/`快捷键快速进行注释

特殊字符：

|   描述   |  字符代码  |
| :------: | :--------: |
|  空格符  |  `&nbsp;`  |
|  小于号  |   `&lt;`   |
|  大于号  |   `&gt;`   |
|   和号   |  `&amp;`   |
|  人民币  |  `&yen;`   |
|   版权   |  `&copy;`  |
| 注册商标 |  `&reg;`   |
|  摄氏度  |  `&deg;`   |
|  正负号  | `&plusmn;` |
|   乘号   |  `times;`  |
|   除号   | `divide;`  |

### 学习成果自我检查
---
* 能够写出HTML基本骨架标签
* 能够写出超链接标签
* 能够写出图片标签并说出alt和title的区别
* 能够写出相对路径的三种形式

### 小技巧
##### 1.使用LiveServer插件
推荐使用vscode来写HTML代码，并通过LiveServer插件实现代码的实时预览。
![LiveServer插件](https://upload-images.jianshu.io/upload_images/5845585-a6dd514d78a4956b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![vscode-live-server-animated-demo.gif](https://upload-images.jianshu.io/upload_images/5845585-806d901cbab23172.gif?imageMogr2/auto-orient/strip)
在Mac中快捷显示网页使用快捷键cmd+L 和cmd+O；
在Windows中快捷显示网页使用快捷键alt+L和alt+O；
##### 2. 使用用户自定义代码片段
vscode使用`!`快速生成的html代码，不是我们想要的，怎么使用自定义按键快速生成自定义代码呢？
点击设置，选择User Snippets,找到html.json,输入以下内容保存即可。
```json
{
	"HTML Template":{
		"prefix": "html_base",
		"body": [
			"<!doctype html>",
			"<html lang=\"zh-cmn-Hans\">",
			"<head>",
			"    <meta charset=\"utf-8\">",
			"<link rel=\"icon\" href=\"./img/favicon.ico\">",
			"    <title>${1:Hello, world!}</title>",
			"</head>",
			"<body>",
			"    ${2:html body here}",
			"</body>",
			"<script>",
			"    ${3://javascript here}",
			"</script>",
			"</html>"
		],
		"description": "Prints tml"
	}
}
```
通过`h`加tab键即可生成自定义HTML代码。
### 总结
本节我们开始了前端知识的第一小节，学习了HTML中最基本的标签的使用方法，下一节我们接着讲解HTML中表格以及表单的相关知识。