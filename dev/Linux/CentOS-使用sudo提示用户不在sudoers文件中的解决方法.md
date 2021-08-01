# CentOS-使用sudo提示用户不在sudoers文件中的解决方法

1. 切换到root用户

   [linux@localhost ~]$ su root

   密码：

   [root@localhost ~]#

2. 查看/etc/sudoers文件权限，如果只读权限，修改为可写权限

    [root@localhost ~]# ll /etc/sudoers

   -r--r-----. 1  root root 4030 12月  10 09:55 /etc/sudoers

    [root@localhost ~]#  chmod 777 /etc/sudoers

   [root@localhost ~]# ls -l /etc/sudoers

   -rwxrwxrwx. 1 root root 4030 12月  10 09:57 /etc/sudoers

3. 修改/etc/sudoers文件，执行命令如下：

   /*username是你的用户名*/

   [root@localhost ~]# echo 'username  ALL=(ALL)   ALL' >> /etc/sudoers  

   或者root权限下输入Visudo 或者 vim /etc/sudoers,找到 root   ALL=(ALL)   ALL的字段,在下一行追加:

   username   ALL=(ALL)   ALL

   5分钟之后密码过期,下次需要重新输入,如果不想如此麻烦，可以用以下方法

   username   ALL=(ALL)   NOPASSWD: ALL

   说明：格式为{用户名   网络中的主机=（执行命令的目标用户）   执行的命令范围}

4. 保存退出，并恢复/etc/sudoers的访问权限为440

   [root@localhost ~]# chmod 440 /etc/sudoers

   [root@localhost ~]# ll /etc/sudoers

   -r--r-----. 1 root root 4030 12月  10 09:59 /etc/sudoers

5. 切换到普通用户，测试用户权限提升功能 