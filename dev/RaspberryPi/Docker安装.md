### Docker安装

#### 修改源

编辑 `/etc/apt/sources.list` 文件，删除原文件所有内容，用以下内容取代：

```powershell
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib rpi
```

编辑 `/etc/apt/sources.list.d/raspi.list` 文件，删除原文件所有内容，用以下内容取代：

```powershell
deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
```

编辑镜像站后，请使用`sudo apt-get update`命令，更新软件源列表，同时检查您的编辑是否正确。

#### 脚本安装

只要输入下面命令，等待安装即可。注：镜像站是阿里云的安装很久没成功。

```powershell
curl -fsSL get.docker.com -o get-docker.sh && sh get-docker.sh
```

安装完成后，如下图所示：

![image-20210205230325649](..\..\images\raspberrypi-docker-install-01.png)



当运行如下命令，将当前pi用户加入docker用户组时，结果不会生效

```powershell
sudo usermod -aG docker pi
```

不用sudo命令，直接运行docker ps会提示：

```
docker: Got permission denied while trying to connect to the Docker daemon socket at un                                                                                          ix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/crea                                                                                          te: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

解决方案：

```powershell
sudo groupadd docker     #添加docker用户组
sudo gpasswd -a $USER docker     #将登陆用户加入到docker用户组中
newgrp docker     #更新用户组
docker ps    #测试docker命令是否可以使用sudo正常使用
```

#### 配置docker加速器

不配加速很慢。

```powershell
通过修改daemon配置文件/etc/docker/daemon.json来使用加速器
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://wgmq6n6j.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

测试一下：

````powershell
docker run hello-world
````

![image-20210205232620348](..\..\images\raspberrypi-docker-install-02.png)

#### 图形化界面安装

```
#下载 Docker 图形化界面 portainer
sudo docker pull portainer/portainer
#创建 portainer 容器
sudo docker volume create portainer_data
#运行 portainer
sudo docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

首次登陆[192.168.0.104:9000](http://192.168.0.104:9000/#/home)，需要设置密码

![image-20210205234245863](..\..\images\raspberrypi-docker-install-03.png)

![image-20210205234330235](..\..\images\raspberrypi-docker-install-04.png)

#### 参考

1. https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/
2. https://download.docker.com/linux/static/stable/armhf/
3. https://shumeipai.nxez.com/2019/05/20/how-to-install-docker-on-your-raspberry-pi.html
4. https://www.cnblogs.com/informatics/p/8276172.html

