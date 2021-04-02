### IDEA-学习

1. 通过官方提供的ideaLearningProject学习，这个项目从欢迎页，通过学习IntelliJ IDEA进入交互式课程。



Virtual Studio配置



#### 基本

##### 1.上下文操作

显示上下文操作 `Alt+Enter`



##### 2.搜索操作

查找操作`Ctrl+Shift+A`

按两次`Shift`

或者

`Ctrl+T`



##### 3.随处搜索

使用Ctrl+Shift+A->转到类...缩小搜索范围

将**项目文件**刷选器切换到**所有位置**

按`Ctrl+Shift+F1`预览可用文档

![image-20210325112213311](..\..\images\idea-course-1.png)

`Ctrl+Alt+Shift+T`查找方法或者全局变量

`Ctrl+Shift+T`查找文件



##### 4.基本补全

输入Ran，按`Ctrl+J`激活“基本补全”，输入后按`Enter`

按`Ctrl+Shift+Enter`补全此语句。

有时按两下`Ctrl+J`查看静态常量或方法建议。

![image-20210325113351634](..\..\images\idea-course-2.png)



#### 编辑器基础知识

##### 1.扩大和缩小代码选择范围

多次 `Ctrl+Alt+向右箭头`

`Ctrl+Alt+向左箭头`缩小到参数



注：快捷键冲突，英特尔显卡关闭旋转屏幕 Ctrl + Alt + F12



##### 2.注释行

`Ctrl+Alt+斜杆`

撤销，再按一次

##### 3.复制和删除行

复制行

`Ctrl+D`

删除行

`Ctrl+Shift+L`

##### 4.移动代码段

`Ctrl+Shift+A` -> 下移行

`Ctrl+Shift+A` -> 上移行

`Ctrl+Alt+Shift+向上箭头` 整个方法上移

`Ctrl+Alt+Shift+向下箭头` 整个方法下移

##### 5.收起

有时您需要收起一段代码以提高可读性。尝试使用`Ctrl+M,S` 收起代码片段。要展开代码区域，请按`Ctrl+M,E`

##### 6.环绕和解开

按`Ctrl+Shift+A`->环绕方式...

选择try/catch/finally模板

通过`Ctrl+Shift+Delete`解开，选择**解开try**项目

![](..\..\images\idea-course-3.gif)

##### 7.多选

按`Alt+Shift+句点`以选择文本光标处的符号。

再次按`Alt+Shift+句点`以选择该符号的下一个匹配项。

按`Alt+Shift+逗号`以选择文件中的所有匹配项。

按`Alt+Shift+分号`以选择文件中的所有匹配项。

键入`td`,将`th`的所有匹配项替换为`td`。

![](..\..\images\idea-course-4.gif)

#### 代码补全

##### 1.基本补全

##### 2.智能类型补全

按`Ctrl+Alt+空格`查看匹配建议的列表。按`Enter`选择第一项。

##### 3.后缀补全

在括号后面键入`.`，以查看后缀补全建议列表。从列表中选择`if`，或在编辑器中键入，然后按`Enter`以补全该语句。

![](..\..\images\idea-course-5.gif)

##### 4.语句补全

按`Ctrl+Shift+Enter`
![](..\..\images\idea-course-6.gif)

##### 5.使用选项卡补全

按`Ctrl+J`显示补全建议。然后按`Tab`。此操作将不会进入插入，而是将替换文本光标处的词语。
![](..\..\images\idea-course-7.gif)

#### 重构

##### 1.重命名

按`Ctrl+R,R`

##### 2.提取变量

按`Ctrl+R,V`
![](..\..\images\idea-course-8.gif)

##### 3.提取方法

按`Ctrl+R,M`
![](..\..\images\idea-course-9.gif)

##### 4.重构菜单

支持多种重构。其中许多都有自己的快捷键。但对于罕见重构，您可以按`Ctrl+Shift+R`并预览部分列表。



#### 编码辅助

##### 1.代码格式

选中代码段，按`Ctrl+Alt+Enter`格式化

未选任何行，按`Ctrl+Alt+Enter`格式化整个文件

##### 2.参数信息

按`Ctrl+Shift+空格`查看方法标签名。 
![](..\..\images\idea-course-10.gif)

##### 3.快速弹出窗口

按`Ctrl+Shift+F1`查看文本光标处的符号文档。

按`Esc`关闭弹出窗口。

按`Alt+F12`以查看文本光标处的符号定义。
![](..\..\images\idea-course-11.gif)

##### 4.编辑器编码辅助

按`Alt+Page Down`转到文件中下一个高亮显示的错误。



#### 导航

##### 1.随处搜索

##### 2.在文件中查找和替换

按`Ctrl+Shift+F`文件中查找

按`Ctrl+Shift+H`文件中替换

##### 3.文件结构

大型源文件可能难以读取和浏览，有时只需要预览。按`Atl+反斜杠`打开文件结构

也可将文件结构显示为工具窗口。使用`Ctrl+Alt+F`将其打开。

![](..\..\images\idea-course-12.gif)

##### 4.声明和用法

按`F12`跳转到方法的声明。

在方法声明中，按`F12`查看汽所有用法，然后选择其中之一。

按`Shift+F12`查看更详细的用法视图。

按`Shift+Esc`隐藏**查找**视图，按`Alt+3`再次打开**查找**视图
![](..\..\images\idea-course-13.gif)

##### 5.继承层次结构

按`Ctrl+Shift+A`->方法层次结构，`Shfit+Esc`隐藏层次结构

要查看类层次结构，按`Ctrl+E,H`

##### 6.最近文件和位置

按`Ctrl+逗号`最近的文件

按`Ctrl+Shift+E`最近访问过得位置

##### 7.下一个/上一个匹配项

按`Ctrl+F`开始在当前文件中执行全文搜索，按上/下键查找匹配项。或`F3`/`Shift+F3`。

#### 运行并调试

##### 1.运行配置

通过绿色标识标记的如何代码均可运行。使用`Ctrl+F5`运行简单示例。

针对每个新的运行，IDEA会创建临时运行配置。如果达到默认限制数据（五个），则自动删除临时配置。我们来将临时配置转换为永久配置。使用运行配置打开下拉菜单。选择**保存‘Sample’配置**。

编辑配置。

##### 2.调试工作流

设置断点，点击左编辑器间距或按`F9`。

要开始调试选定的运行配置，点甲虫或`Alt+F5`

