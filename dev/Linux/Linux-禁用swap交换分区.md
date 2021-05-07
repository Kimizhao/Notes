### Linux-禁用swap交换分区

一、不重启电脑，禁用启用swap，立刻生效
禁用命令
sudo swapoff -a
启用命令
sudo swapon -a
查看交换分区的状态
sudo free -m
二、重新启动电脑，永久禁用Swap
把根目录文件系统设为可读写
sudo mount -n -o remount,rw /
用vi修改/etc/fstab文件，在swap分区这行前加 # 禁用掉，保存退出
vi /etc/fstab
i      #进入insert 插入模式
:wq   #保存退出

重新启动电脑，使用free -m查看分区状态
reboot
sudo free -m