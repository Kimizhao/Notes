### 树莓派-花生棒搭建私有git服务器ssh协议

#### 第一步 [安装git和ssh](https://blog.csdn.net/cherisegege/article/details/75673613)

#### 第二步**添加端口映射（DDNS）**

1.花生棒开机

2.服务添加内网穿透

![Image](D:\700-repos\github\Kimizhao\Notes\images\raspberrypi-git-01.png)

**第三步 通过SSH协议，克隆代码**

正确操作：

git clone ssh://**[git@147405.cicp.net](mailto:git@147405.cicp.net)**:48821/home/git/test.git
失败的操作有：git clone [git@147405.cicp.net](mailto:git@147405.cicp.net)**:48821/home/git/test.git**

git clone [git@147405.cicp.net](mailto:git@147405.cicp.net)**:/home/git/test.git**
**git clone ssh://147405.**[cicp.net:48821/home/git/test.git](http://cicp.net:48821/home/git/test.git)缺少用户名



**为何多数 git 教程里添加远程库默认推荐 ssh，而不是智能 http(s) 协议（无需ssh密钥）？**git支持4种协议，包括本地协议、HTTP协议、Git协议、SSH协议。我们继续看看Git协议。https://www.zhihu.com/question/25715233/answer/95800632