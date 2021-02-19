### Linux-crontab命令
​	crontab命令常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中，以供之后读取和执行。该词来源于希腊语 chronos(χρνο)，原意是时间。常，crontab储存的指令被守护进程激活， crond常常在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为cron jobs。

### 使用说明

​	只有root用户和crontab文件的所有者才能编辑定时任务，因此如果以pi用户登录，不要忘记加上`sudo`。`-e`参数表示编辑(edit)。 
​	`sudo crontab -e` 
​	进入编辑以后需要按照一定的格式写入所需执行的命令和重复的时间。格式如下： 
​	`	m h dom mon dow command` 
​	依次是分钟(minute)、小时(hour)、几号(day of month)、月份(month)、星期几(day of week)、命令。 
时间可以是一个数字，表示在这个时刻执行，也可以是星号(*)，表示不做限制、在任意时刻都执行。 
查看所有的定时任务可以使用`-l`参数，表示列出(list)的含义 
​	`crontab -l`

### 用法举例

每天0点1分执行贴吧签到脚本 
	`1 0 * * * python qiandao.py` 
在每周日的7点更新系统
	`0 7 * * 1 apt-get update && sudo apt-get upgrade -y`