### Docker-在Ubuntu下如何将docker数据目录移动到的另一个目录

Docker 是一个流行的容器管理平台，可以极大地加快您的开发工作流。它可以作为主流 Linux 发行版的一个包提供，包括 Ubuntu。

Docker 使用的标准数据目录是/var/lib/docker，由于该目录将存储所有图像、卷等，因此可以在相对较短的时间内变得相当大。

如果希望将 docker 数据目录移动到另一个位置，可以按照以下简单步骤操作。

#### 环境

Ubuntu 16.04 LTS 

Docker 17.05.0-ce

#### 1.停止docker

```
sudo service docker stop
```

#### 2. 添加一个配置文件来告诉 docker 守护进程数据目录的位置

使用您喜欢的文本编辑器，在/etc/docker 目录下添加一个名为 daemon.json 的文件:

```
{ 
   "data-root": "/path/to/your/docker" 
}
```

当然，您应该使用新的 docker 数据目录的路径自定义位置`/path/to/your/docker`。这里使用`/home/zzh/dockerimages`。

#### 3. 将当前数据目录复制到新目录

```
sudo rsync -aP /var/lib/docker/ /path/to/your/docker
```

这里用

```
sudo rsync -aP /var/lib/docker/ /home/zzh/dockerimages
```

#### 4. 重命名旧的 docker 目录

```
sudo mv /var/lib/docker /var/lib/docker.old
```

#### 5. 重新加载 docker 守护进程

```
sudo systemctl daemon-reload
```

#### 6.重新启动 docker 守护进程

```
sudo service docker start
```

#### 7.测试

如果一切正常，您应该看不到使用 docker 容器的区别。当您确定 docker 守护进程正确使用新目录时，可以删除旧的数据目录。

```
sudo rm -rf /var/lib/docker.old
```



#### 参考

1.https://www.guguweb.com/2019/02/07/how-to-move-docker-data-directory-to-another-location-on-ubuntu/

2.https://blog.csdn.net/EasternUnbeaten/article/details/80463837

