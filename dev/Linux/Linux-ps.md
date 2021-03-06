### Linux-ps

linux上进程有5种状态:
1. 运行(正在运行或在运行队列中等待)
2. 中断(休眠中, 受阻, 在等待某个条件的形成或接受到信号)
3. 不可中断(收到信号不唤醒和不可运行, 进程必须等待直到有中断发生)
4. 僵死(进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放)
5. 停止(进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行)


1）ps a 显示现行终端机下的所有程序，包括其他用户的程序。
2）ps -A   显示所有程序。
3）ps c    列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示。
4）ps -e  此参数的效果和指定"A"参数相同。
5）ps e   列出程序时，显示每个程序所使用的环境变量。
6）ps f    用ASCII字符显示树状结构，表达程序间的相互关系。
7）ps -H    显示树状结构，表示程序间的相互关系。
8）ps -N   显示所有的程序，除了执行ps指令终端机下的程序之外。
9）ps s     采用程序信号的格式显示程序状况。
10）ps S     列出程序时，包括已中断的子程序资料。
11）ps -t <终端机编号> 　指定终端机编号，并列出属于该终端机的程序的状况。
12）ps u 　 以用户为主的格式来显示程序状况。
13）ps x 　 显示所有程序，不以终端机来区分。
14）ps -l     较长,较详细的显示该PID的信息。



https://blog.csdn.net/hanner_cheung/article/details/6081440