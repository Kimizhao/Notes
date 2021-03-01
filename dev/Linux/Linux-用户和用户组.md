### Linux-用户和用户组

用户帐号信息由
/etc/passwd
/etc/shadow
文件共同维护

 #### 组操作
1.创建组

groupadd test

增加一个test组

2.修改组

groupmod -n test2  test

将test组的名子改成test2

3.删除组

groupdel test2

删除 组test2

4.查看组

a），查看当前登录用户所在的组 groups，查看apacheuser所在组groups apacheuser

b），查看所有组 cat /etc/group

c），有的linux系统没有/etc/group文件的，这个时候看下面的这个方法

cat /etc/passwd |awk -F [:] ‘{print $4}’ |sort|uniq | getent group |awk -F [:] ‘{print $1}’

这里用到一个命令是getent,可以通过组ID来查找组信息,如果这个命令没有的话,那就很难查找,系统中所有的组了.

#### 用户操作

1.增加用户

查看复制打印?
[root@cms01 mytest]# useradd –help
Usage: useradd [options] LOGIN

Options:
-b, –base-dir BASE_DIR       设置基本路径作为用户的登录目录
-c, –comment COMMENT         对用户的注释
-d, –home-dir HOME_DIR       设置用户的登录目录
-D, –defaults                改变设置
-e, –expiredate EXPIRE_DATE 设置用户的有效期
-f, –inactive INACTIVE       用户过期后，让密码无效
-g, –gid GROUP               使用户只属于某个组
-G, –groups GROUPS           使用户加入某个组
-h, –help                    帮助
-k, –skel SKEL_DIR           指定其他的skel目录
-K, –key KEY=VALUE           覆盖 /etc/login.defs 配置文件
-m, –create-home             自动创建登录目录
-l,                           不把用户加入到lastlog文件中
-M,                           不自动创建登录目录
-r,                           建立系统账号
-o, –non-unique              允许用户拥有相同的UID
-p, –password PASSWORD       为新用户使用加密密码
-s, –shell SHELL             登录时候的shell
-u, –uid UID                 为新用户指定一个UID
-Z, –selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
[root@cms01 mytest]# useradd --help
Usage: useradd [options] LOGIN

Options:
 -b, --base-dir BASE_DIR       设置基本路径作为用户的登录目录
 -c, --comment COMMENT         对用户的注释
 -d, --home-dir HOME_DIR       设置用户的登录目录
 -D, --defaults                改变设置
 -e, --expiredate EXPIRE_DATE 设置用户的有效期
 -f, --inactive INACTIVE       用户过期后，让密码无效
 -g, --gid GROUP               使用户只属于某个组
 -G, --groups GROUPS           使用户加入某个组
 -h, --help                    帮助
 -k, --skel SKEL_DIR           指定其他的skel目录
 -K, --key KEY=VALUE           覆盖 /etc/login.defs 配置文件
 -m, --create-home             自动创建登录目录
 -l,                           不把用户加入到lastlog文件中
 -M,                           不自动创建登录目录
 -r,                           建立系统账号
 -o, --non-unique              允许用户拥有相同的UID
 -p, --password PASSWORD       为新用户使用加密密码
 -s, --shell SHELL             登录时候的shell
 -u, --uid UID                 为新用户指定一个UID
 -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mappinguseradd test

passwd test

增加用户test，有一点要注意的，useradd增加一个用户后，不要忘了给他设置密码，不然不能登录的。

2.修改用户

usermod -d /home/test -G test2 test

将test用户的登录目录改成/home/test，并加入test2组，注意这里是大G。

gpasswd -a test test2 将用户test加入到test2组
gpasswd -d test test2 将用户test从test2组中移出

3.删除用户

userdel test

将test用户删除

4.查看用户

a）查看当前登录用户

[root@cms01 ~]# w
[root@cms01 ~]# who

b）查看自己的用户名

[root@cms01 ~]# whoami

c）查看单个用户信息

[root@cms01 ~]# finger apacheuser
[root@cms01 ~]# id apacheuser

d）查看用户登录记录

[root@cms01 ~]# last 查看登录成功的用户记录
[root@cms01 ~]# lastb 查看登录不成功的用户记录

e）查看所有用户

[root@cms01 ~]# cut -d : -f 1 /etc/passwd
[root@cms01 ~]# cat /etc/passwd |awk -F \: ‘{print $1}’