# GitLab CI/CD

## 开始



## [概念](https://docs.gitlab.com/ee/ci/introduction/)



## [Docker集成](https://docs.gitlab.com/ee/ci/docker/)

### 



## [示例](https://docs.gitlab.com/ee/ci/examples/)

[Sprint](https://gitlab.com/gitlab-examples/spring-gitlab-cf-deploy-demo/-/tree/master)



## 遇到问题

1.gitlab-runner运行jobs时提示找不到npm

默认启动时用gitlab-runner用户，访问不到npm命令

npm用root用户安装



2.gitlab-runner卡住，说找不到tag，重新部署一下

注册gitlab-runner时，记住tag。



3.npm ERR! gyp info spawn /usr/bin/python

需要安装python



4.gitlab CI 失败一次后 fatal: git fetch-pack: expected shallow list

git版本低



## [安装gitlab-runner](https://docs.gitlab.com/runner/install/linux-manually.html#install-1)

1. 下载

```
$ sudo curl -L --output /usr/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```

2. 分配执行权限:

```
sudo chmod +x /usr/bin/gitlab-runner
```

3. Create a GitLab CI user:

```
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

4. Install and run as service:

```
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

5. [Register a runner](https://docs.gitlab.com/runner/register/index.html)

```
sudo gitlab-runner register
```

```
$ sudo gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=18130 revision=8925d9a0 version=14.1.0
Running in system-mode.

Enter the GitLab instance URL (for example, https://gitlab.com/):
http://192.168.10.74/
Enter the registration token:
2J4WMXYArgfaQ4iGNkEz
Enter a description for the runner:
[hadoop69]: hadoop69
Enter tags for the runner (comma-separated):
stone
Registering runner... succeeded                     runner=2J4WMXYA
Enter an executor: docker-ssh, parallels, ssh, docker+machine, kubernetes, custom, docker, shell, virtualbox, docker-ssh+machine:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```



默认运行

```
/usr/bin/gitlab-runner run --working-directory /home/gitlab-runner --config /etc/gitlab-runner/config.toml --service gitlab-runner --user gitlab-runner
```

## 升级

### 1. 停止

```
sudo gitlab-runner stop
```



### 2.下载

```
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```



### 3.赋予执行权限

```
sudo chmod +x /usr/local/bin/gitlab-runner
```



### 4.启动

```
sudo gitlab-runner start
```



注：需要运行docker，假如用户组

```
# 将test用户加入docker用户组，这样就不需要每次在docker命令前加sudo
sudo usermod -aG docker gitlab-runner
# 更新用户组
newgrp docker
```



## 参考

[gitlab ci 部署vue项目](https://www.jianshu.com/p/3c0cbb6c2936)

[GitLab的CI/CD中 声明和使用变量的三种方式](https://blog.csdn.net/github_35631540/article/details/107663922)

[gitlab部署vue项目](https://www.jianshu.com/p/1817c9329aa6)

[Gitlab CI 使用高级技巧](https://www.jianshu.com/p/3c0cbb6c2936)



