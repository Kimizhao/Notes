超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。

## [简介](https://www.runoob.com/html/html-intro.html)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>摄星科技</title>
    </head>
    <body>
        <h1>我的第一个标题</h1>
        <p>我的第一个段落。</p>
        <div></div>
    </body>
</html>
```

**实例解析**

<!DOCTYPE html> 声明为 HTML5 文档；<html> 元素是 HTML 页面的根元素；<head> 元素包含了文档的元（meta）数据，如 <meta charset="utf-8"> 定义网页编码格式为 utf-8；<title> 元素描述了文档的标题；<body> 元素包含了可见的页面内容；<h1> 元素定义一个大标题；<p> 元素定义一个段落

注：在浏览器的页面上使用键盘上的 F12 按键开启调试模式，就可以看到组成标签。



## HTML 元素

| 开始标签 *             | 元素内容     | 结束标签 * |
| :--------------------- | :----------- | :--------- |
| <p>                    | 这是一个段落 | </p>       |
| <a href="default.htm"> | 这是一个链接 | </a>       |
| <br>                   | 换行         |            |

开始标签常被称为**起始标签（opening tag），**结束标签常称为**闭合标签（closing tag）**。
**HTML 元素语法**

- HTML 元素以**开始标签**起始

- HTML 元素以**结束标签**终止

- ** 元素的内容**是开始标签与结束标签之间的内容

- 某些 HTML 元素**具有空内容（empty content）**

- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）

- 大多数 HTML 元素可拥有**属性**

**提示**

不要忘记结束标签

没有内容的元素叫空元素

使用小写标签



## HTML 属性

属性是 HTML 元素提供的附加信息。

- HTML 元素可以设置属性
- 属性可以在元素中添加附加信息
- 属性一般描述于开始标签
- 属性总是以名称/值对的形式出现，比如：name="value"。

**属性实例**

HTML 链接由 <a> 标签定义。链接的地址在 href 属性中指定：

```html
<a href="http://www.runoob.com">这是一个链接</a>
```



## HTML区块

HTML 可以通过 <div> 和 <span>将元素组合起来。其中， <div> 元素是块级(block-level)元素，定义了文档的区域，它可用于组合其他 HTML 元素的容器。<span> 元素是内联(inline)元素，用来组合文档中的行内元素，可用作文本的容器。

**HTML 区块元素**
大多数 HTML 元素被定义为块级元素或内联元素。

块级元素在浏览器显示时，通常会以**新行**来开始（和结束）。

实例: <h1>, <p>, <ul>, <table>

**HTML 内联元素**
内联元素在显示时通常**不会以新行**开始。

实例: <b>, <td>, <a>, <img>

## HTML 全局属性

class 规定元素的一个或多个类名（引用样式表中的类）

style 规定元素的行内CSS样式