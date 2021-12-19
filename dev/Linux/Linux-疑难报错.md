### sudo用户找不到环境变量 sudo找不到/usr/local/bin 下的执行文件

出于安全方面的考虑，使用sudo执行命令将在一个最小化的环境中执行，环境变量都重置成默认状态。

所以PATH这个变量不包括用户自定义设置的内容，如找不到/usr/local/bin/下面的命令
在sudo用户的主目录里的.bashrc中添加如下内容即可解决

$ vim ~/.bashrc 

#在最下面添加如下一行
alias sudo="sudo env PATH=$PATH"
source ~/.bashrc
之后sudo就有可以找到/usr/local/bin下面的命令了

