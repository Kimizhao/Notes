# GitLab CI/CD

## 开始



## [概念](https://docs.gitlab.com/ee/ci/introduction/)



## [Docker集成](https://docs.gitlab.com/ee/ci/docker/)

### 



## [示例](https://docs.gitlab.com/ee/ci/examples/)

[Sprint](https://gitlab.com/gitlab-examples/spring-gitlab-cf-deploy-demo/-/tree/master)



### 常用命令

```
# 启动
gitlab-ctl start

# 配置
gitlab-ctl reconfigure

# 停止
gitlab-ctl stop
#查看日志
gitlab-ctl tail
```



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



# 关闭gitlab中的prometheus

编辑/etc/gitlab/gitlab.rb
添加或查找并取消注释以下行
prometheus_monitoring[‘enable’]，确保将其设置为 ：false

```
vim /etc/gitlab/gitlab.rb
```

prometheus_monitoring['enable'] = false

保存并重新配置GitLab

```
gitlab-ctl reconfigure
```



## 参考

[gitlab ci 部署vue项目](https://www.jianshu.com/p/3c0cbb6c2936)

[GitLab的CI/CD中 声明和使用变量的三种方式](https://blog.csdn.net/github_35631540/article/details/107663922)

[gitlab部署vue项目](https://www.jianshu.com/p/1817c9329aa6)

[Gitlab CI 使用高级技巧](https://www.jianshu.com/p/3c0cbb6c2936)





## 错误

### 1.Starting Prometheus

fd和vm超出

临时关闭prometheus

```
2022-01-18_06:17:30.18220 level=info ts=2022-01-18T06:17:30.181Z caller=main.go:329 msg="Starting Prometheus" version="(version=2.12.0, branch=master, revision=)"
2022-01-18_06:17:30.18225 level=info ts=2022-01-18T06:17:30.182Z caller=main.go:330 build_context="(go=go1.12.13, user=GitLab-Omnibus, date=)"
2022-01-18_06:17:30.18225 level=info ts=2022-01-18T06:17:30.182Z caller=main.go:331 host_details="(Linux 3.10.0-1062.el7.x86_64 #1 SMP Wed Aug 7 18:08:02 UTC 2019 x86_64 registry.aidipper.com (none))"
2022-01-18_06:17:30.18240 level=info ts=2022-01-18T06:17:30.182Z caller=main.go:332 fd_limits="(soft=50000, hard=50000)"
2022-01-18_06:17:30.18340 level=info ts=2022-01-18T06:17:30.182Z caller=main.go:333 vm_limits="(soft=unlimited, hard=unlimited)"
2022-01-18_06:17:30.18692 panic: runtime error: slice bounds out of range
2022-01-18_06:17:30.18694
2022-01-18_06:17:30.18694 goroutine 1 [running]:
2022-01-18_06:17:30.18695 github.com/prometheus/prometheus/promql.parseBrokenJson(0xc0006f3000, 0x4e21, 0x4e21, 0x2a74ce0, 0xc000649bf0, 0x0, 0x0, 0xc000730330)
2022-01-18_06:17:30.18695       /var/cache/omnibus/src/prometheus/src/github.com/prometheus/prometheus/promql/query_logger.go:45 +0x131
2022-01-18_06:17:30.18695 github.com/prometheus/prometheus/promql.logUnfinishedQueries(0xc000730330, 0x2e, 0x4e21, 0x2a74ce0, 0xc000649bf0)
2022-01-18_06:17:30.18697       /var/cache/omnibus/src/prometheus/src/github.com/prometheus/prometheus/promql/query_logger.go:70 +0x552
2022-01-18_06:17:30.18700 github.com/prometheus/prometheus/promql.NewActiveQueryTracker(0x7fffd55afe8d, 0x1f, 0x14, 0x2a74ce0, 0xc000649bf0, 0x2a74ce0)
2022-01-18_06:17:30.18704       /var/cache/omnibus/src/prometheus/src/github.com/prometheus/prometheus/promql/query_logger.go:108 +0x131
2022-01-18_06:17:30.18704 main.main()
2022-01-18_06:17:30.18705       /var/cache/omnibus/src/prometheus/src/github.com/prometheus/prometheus/cmd/prometheus/main.go:361 +0x52bd

==> /var/log/gitlab/nginx/gitlab_access.log <==
192.168.10.74 - - [18/Jan/2022:14:17:30 +0800] "POST /api/v4/jobs/request HTTP/1.1" 204 0 "" "gitlab-runner 14.1.0 (14-1-stable; go1.13.8; linux/amd64)"
192.168.10.69 - - [18/Jan/2022:14:17:30 +0800] "POST /api/v4/jobs/request HTTP/1.1" 204 0 "" "gitlab-runner 14.1.0 (14-1-stable; go1.13.8; linux/amd64)"
192.168.10.68 - - [18/Jan/2022:14:17:30 +0800] "POST /api/v4/jobs/request HTTP/1.1" 204 0 "" "gitlab-runner 14.1.0 (14-1-stable; go1.13.8; linux/amd64)"

==> /var/log/gitlab/prometheus/current <==
```

