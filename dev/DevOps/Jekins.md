# Jekins

Jenkins 下载
https://www.jenkins.io/zh/download/

安装Jenkins
https://www.jenkins.io/zh/doc/book/installing/#prerequisites

Docker中安装
1.下载 jenkinsci/blueocean 镜像
2.使用以下docker run 命令将其作为Docker中的容器运行

```
docker run \
  -u root \
  -d \
  -p 9090:8080 \
  -p 50000:50000 \
  -v /home/zzh/jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
```

安装后设置向导
https://www.jenkins.io/zh/doc/book/installing/#setup-wizard