## [CentOS7-修改为国内yum源](https://www.cnblogs.com/cerana/p/11179728.html)

1. 备份源yum源
   如果是国内下载的CentOS很可能国内YUM源已经设置好了。
   备份/etc/yum.repos.d/下的*.repo文件。
2. 在CentOS中配置使用网易和阿里的开源镜像
   wget http://mirrors.aliyun.com/repo/Centos-7.repo
   wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
   或者手动下载repo文件并上传到/etc/yum.repos.d/目录
3. 清除系统yum缓存并生成新的yum缓存
   yum clean all # 清除系统所有的yum缓存
   yum makecache # 生成yum缓存
4. 安装epel源
   yum list | grep epel-release
   yum install -y epel-release
5. 再次清除系统yum缓存，并重新生成新的yum缓存
   yum clean all
   yum makecache
6. 查看系统可用的yum源和所有的yum源
   yum repolist enabled
   yum repolist all
7. 测试安装
   yum install openssh-server