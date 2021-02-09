## 打包jar

**命令方式**

```
mvn   clean   package   -pl   这里是项目名称   -am
```

**IDEA界面**

在idea侧边栏（默认在右边栏）找到Maven窗口，找到要发布的项目，点击项目名称左边的三角，展开找到Lifecycle，展开找到package，右键“Run Maven Build”或者“Run 项目名称”按钮就可以了。然后等着底部的控制台输出打包过程日志，打包完成会显示.jar包所在位置。



## Maven国内加速，修改镜像源

http://www.xiegangd.com/article/153440229013286

### 如何修改镜像源

可以使用阿里巴巴提供的 maven 镜像源 http://maven.aliyun.com/nexus/content/groups/public

#### a). 配置只在当前项目生效

在 pom.xml 文件内添加以下配置

```xml
<repositories>
    <repository>
        <id>ali-maven</id>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </repository>
</repositories>
```

#### b). 配置全局生效

修改 settings.xml 文件

找到 mirrors 标签，在里面加入以下内容

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

