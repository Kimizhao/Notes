### jekins-部署sprintboot应用

[使用Jenkins一键打包部署SpringBoot应用，就是这么6！](https://mp.weixin.qq.com/s/tQqvgSc9cHBtnqRQSbI4aw)



#### 安装jekins

#### 配置远程执行ssh脚本

#### 将代码上传到Git仓库

修改dockerHost，注意这里是docker远程访问地址，端口默认是2375

PS：不是docker私有镜像仓库地址

#### 执行脚本mall-tiny-jenkins.sh，因为宿主机的8088端口被占用，改为8188

```
#!/usr/bin/env bash
app_name='mall-tiny-jenkins'
docker stop ${app_name}
echo '----stop container----'
docker rm ${app_name}
echo '----rm container----'
docker run -p 8188:8088 --name ${app_name} \
--link mysql8.0.22 \
-v /etc/localtime:/etc/localtime \
-v /home/zzh/mall-data/app/${app_name}/logs:/var/logs \
-d mall-tiny/${app_name}:1.0-SNAPSHOT
echo '----start container----'
```

![image-20210330144915538](..\..\images\jekins-sprintboot-1.png)

