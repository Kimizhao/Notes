## JavaScript 对象

在 JavaScript中，几乎所有的事物都是对象。

在 JavaScript 中，对象是非常重要的，当你理解了对象，就可以了解 JavaScript 。	



## 对象定义

你可以使用字符来定义和创建 JavaScript 对象:

## 对象属性

可以说 "JavaScript 对象是变量的容器"。

但是，我们通常认为 "JavaScript 对象是键值对的容器"。

键值对通常写法为 **name : value** (键与值以冒号分割)。

键值对在 JavaScript 对象通常称为 **对象属性**。

## 访问对象属性

你可以通过两种方式访问对象属性:

实例1

```
person.lastName;
```

实例2

```
person["lastName"];
```



## 对象方法

对象的方法定义了一个函数，并作为对象的属性存储。

对象方法通过添加 () 调用 (作为一个函数)。

该实例访问了 person 对象的 fullName() 方法:

```
name = person.fullName();
```

如果你要访问 person 对象的 fullName 属性，它将作为一个定义函数的字符串返回：

```
name = person.fullName;
```

JavaScript 对象是属性和方法的容器。

## 访问对象方法

你可以使用以下语法创建对象方法：

```
methodName : function() {
    // 代码 
}
```

你可以使用以下语法访问对象方法：

```
objectName.methodName()
```

通常 fullName() 是作为 person 对象的一个方法， fullName 是作为一个属性。

如果使用 fullName 属性，不添加 **()**, 它会返回函数的定义：

```
objectName.methodName
```

有多种方式可以创建，使用和修改 JavaScript 对象。

同样也有多种方式用来创建，使用和修改属性和方法。



# JavaScript 作用域

作用域是可访问变量的集合。

------

## JavaScript 作用域

在 JavaScript 中, 对象和函数同样也是变量。

**在 JavaScript 中, 作用域为可访问变量，对象，函数的集合。**

JavaScript 函数作用域: 作用域在函数内修改。

------

## JavaScript 局部作用域

变量在函数内声明，变量为局部作用域。

局部变量：只能在函数内部访问。

```
// 此处不能调用 carName 变量
function myFunction() {
    var carName = "Volvo";
    // 函数内可调用 carName 变量
}
```

因为局部变量只作用于函数内，所以不同的函数可以使用相同名称的变量。

局部变量在函数开始执行时创建，函数执行完后局部变量会自动销毁。

------

## JavaScript 全局变量

变量在函数外定义，即为全局变量。

全局变量有 **全局作用域**: 网页中所有脚本和函数均可使用。 

```
var carName = " Volvo";
 
// 此处可调用 carName 变量
function myFunction() {
    // 函数内可调用 carName 变量
}
```

如果变量在函数内没有声明（没有使用 var 关键字），该变量为全局变量。

以下实例中 carName 在函数内，但是为全局变量。

```
// 此处可调用 carName 变量
 
function myFunction() {
    carName = "Volvo";
    // 此处可调用 carName 变量
}
```

## JavaScript 变量生命周期

JavaScript 变量生命周期在它声明时初始化。

局部变量在函数执行完毕后销毁。

全局变量在页面关闭后销毁。

------

## 函数参数

函数参数只在函数内起作用，是局部变量。

------

## HTML 中的全局变量

在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象。

```
//此处可使用 window.carName
 
function myFunction() {
    carName = "Volvo";
}
```



# JavaScript 事件

HTML 事件是发生在 HTML 元素上的事情。

当在 HTML 页面中使用 JavaScript 时， JavaScript 可以触发这些事件。

## HTML 事件

HTML 事件可以是浏览器行为，也可以是用户行为。

以下是 HTML 事件的实例：

- HTML 页面完成加载
- HTML input 字段改变时
- HTML 按钮被点击

通常，当事件发生时，你可以做些事情。

在事件触发时 JavaScript 可以执行一些代码。

HTML 元素中可以添加事件属性，使用 JavaScript 代码来添加 HTML 元素。

单引号:

<*some-HTML-element* *some-event*=**'*****JavaScript 代码*****'**>

双引号:

<*some-HTML-element* *some-event*=**"*****JavaScript 代码*****"**>

在以下实例中，按钮元素中添加了 onclick 属性 (并加上代码):

```
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
```

以上实例中，JavaScript 代码将修改 id="demo" 元素的内容。

在下一个实例中，代码将修改自身元素的内容 (使用 this.innerHTML):

```
<button onclick="this.innerHTML=Date()">现在的时间是?</button>
```

​	JavaScript代码通常是几行代码。比较常见的是通过事件属性来调用：

```
<button onclick="displayDate()">现在的时间是?</button>
```

# JavaScript let 和 const

### ECMAScript 2015(ECMAScript 6)

ES2015(ES6) 新增加了两个重要的 JavaScript 关键字: **let** 和 **const**。

let 声明的变量只在 let 命令所在的代码块内有效。

const 声明一个只读的常量，一旦声明，常量的值就不能改变。

在 ES6 之前，JavaScript 只有两种作用域： **全局变量** 与 **函数内的局部变量**。





# JavaScript Promise

在学习本章节内容前，你需要先了解什么是异步编程，可以参考：[JavaScript 异步编程](https://www.runoob.com/js/javascript-async.html)

Promise 是一个 ECMAScript 6 提供的类，目的是更加优雅地书写复杂的异步任务。





# [JavaScript 代码规范](https://www.runoob.com/js/js-conventions.html)

------

所有的 JavaScript 项目适用同一种规范。

------

## JavaScript 代码规范

代码规范通常包括以下几个方面:

- 变量和函数的命名规则
- 空格，缩进，注释的使用规则。
- 其他常用规范……

规范的代码可以更易于阅读与维护。

代码规范一般在开发前规定，可以跟你的团队成员来协商设置。



# JavaScript 函数定义

JavaScript 使用关键字 **function** 定义函数。

函数可以通过声明定义，也可以是一个表达式。

## 函数声明

在之前的教程中，你已经了解了函数声明的语法 :

```
function functionName(parameters) {
  执行的代码
}
```

函数声明后不会立即执行，会在我们需要的时候调用到。

## 函数是对象

在 JavaScript 中使用 **typeof** 操作符判断函数类型将返回 "function" 。

但是JavaScript 函数描述为一个对象更加准确。

JavaScript 函数有 **属性** 和 **方法**。

## 箭头函数

ES6 新增了箭头函数。

箭头函数表达式的语法比普通函数表达式更简洁。



# JavaScript HTML DOM

------

通过 HTML DOM，可访问 JavaScript HTML 文档的所有元素。

------

## HTML DOM (文档对象模型)

当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）。