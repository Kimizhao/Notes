

# GitLab简明教程

## 1.注册登陆

WEB地址：192.168.10.74

Sign in 输入账号、密码

Register 注册，一般使用企业内部邮件注册。

![1-login](..\..\images\gitlab\1-login.png)



## 2.仪表板DashBoard

![2-dashboard](..\..\\images\gitlab\2-dashboard.png)





## 3.设置

登陆系统成功后，第一步进行偏好设置。

把语言改成**中文**。

右上角->点击“用户头像”->“Settings”->"User Settings"->"Preferences"->"Localization"=>"Language"

![1-settings](..\..\\images\gitlab\1-settings.png)

其他根据自己偏好设置，主题。

![1-settings-2](..\..\images\gitlab\1-settings-2.png)



## 4.项目

### 4.1 新建项目

在首页仪表板，选择【新建项目】

![3-new-project](..\..\images\gitlab\3-new-project.png)

表单页面如：

![3-new-project-2](..\..\\images\gitlab\3-new-project-2.png)

包含有三种标签页：

1.Blank project：从空项目创建

2.Create from template：从模板创建，如Spring、Node、.Net Core、Android等

3.Import project：从外部系统导入，如GitHub、Gitlab.com、Google Code等



### 4.2 新建demo项目

从空项目创建为例。

![3-new-project-demo](..\..\\images\gitlab\3-new-project-demo.png)

项目名称：demo

项目URL：可选择用户组或用户名

项目标识串：demo，输入项目名称时自动填入

项目描述(可选)：新建demo项目、测试

可视访问：个人项目勾选**私有**

最后点击“Create project”。

demo创建成功。

![3-new-project-demo-1](..\..\images\gitlab\3-new-project-demo-1.png)



### 4.3 推送上传项目

根据以下命令行指引，从计算机中上传现有文件。

##### Git 全局设置

```shell
git config --global user.name "xxx"
git config --global user.email "xxx@yyy.com"
```

##### 创建一个新仓库

```shell
git clone git@192.168.10.74:ygai/demo.git
cd demo
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

##### 推送现有文件夹

```shell
cd existing_folder
git init
git remote add origin git@192.168.10.74:ygai/demo.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

##### 推送现有的 Git 仓库

```shell
cd existing_repo
git remote rename origin old-origin
git remote add origin git@192.168.10.74:ygai/demo.git
git push -u origin --all
git push -u origin --tags
```



以**创建一个新的仓库**为例：

![3-new-project-demo-2](..\..\\images\gitlab\3-new-project-demo-2.png)

![3-new-project-demo-3](..\..\images\gitlab\3-new-project-demo-3.png)

## 5. 项目设置

​	在本项目左侧导航【设置】

### 5.1 通用

项目名称、主题、头像

可见性、项目功能、权限

合并请求

徽章

高级 管家、导出、路径、转移、删除、归档

### 5.2 成员

因为是私有项目，只有项目创建人才有可视权限。

邀请新成员或另一个群组加入。

选择成员名

选择角色权限，从低到高。

Guest < Reporter < Developer < Maintainer < Owner。

![3-new-project-demo-4](..\..\images\gitlab\3-new-project-demo-4.png)

### 5.3 集成

当项目中发生事件时，Webhooks 可用于绑定事件。

项目服务允许您将 GitLab 与其他应用程序集成。包括：

| 服务                                                         | 描述                                                         |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| **[ Asana](http://192.168.10.74/ygai/demo/-/services/asana/edit)** | Asana - 无需电子邮件实现团队协作                             |      |
| [**Assembla**](http://192.168.10.74/ygai/demo/-/services/assembla/edit) | Project Management Software (Source Commits Endpoint)        |      |
| [**Atlassian Bamboo CI**](http://192.168.10.74/ygai/demo/-/services/bamboo/edit) | 一个持续集成和构建服务器                                     |      |
| [**Bugzilla**](http://192.168.10.74/ygai/demo/-/services/bugzilla/edit) | Bugzilla issue tracker                                       |      |
| [**Buildkite**](http://192.168.10.74/ygai/demo/-/services/buildkite/edit) | Continuous integration and deployments                       |      |
| [**Campfire**](http://192.168.10.74/ygai/demo/-/services/campfire/edit) | Simple web-based real-time group chat                        |      |
| [**Custom Issue Tracker**](http://192.168.10.74/ygai/demo/-/services/custom_issue_tracker/edit) | Custom issue tracker                                         |      |
| [**Discord 通知**](http://192.168.10.74/ygai/demo/-/services/discord/edit) | 在 Discord 中接收事件通知                                    |      |
| [**Drone CI**](http://192.168.10.74/ygai/demo/-/services/drone_ci/edit) | Drone is a Continuous Integration platform built on Docker, written in Go |      |
| [**Flowdock**](http://192.168.10.74/ygai/demo/-/services/flowdock/edit) | Flowdock是一个面向技术团队协作的Web应用程序。                |      |
| [**Hangouts Chat**](http://192.168.10.74/ygai/demo/-/services/hangouts_chat/edit) | Receive event notifications in Google Hangouts Chat          |      |
| [**HipChat**](http://192.168.10.74/ygai/demo/-/services/hipchat/edit) | Private group chat and IM                                    |      |
| [**Irker (IRC gateway)**](http://192.168.10.74/ygai/demo/-/services/irker/edit) | Send IRC messages, on update, to a list of recipients through an Irker gateway. |      |
| [**JetBrains TeamCity CI**](http://192.168.10.74/ygai/demo/-/services/teamcity/edit) | A continuous integration and build server                    |      |
| [**Jira**](http://192.168.10.74/ygai/demo/-/services/jira/edit) | Jira 议题跟踪                                                |      |
| [**Mattermost notifications**](http://192.168.10.74/ygai/demo/-/services/mattermost/edit) | Receive event notifications in Mattermost                    |      |
| [**Mattermost slash commands**](http://192.168.10.74/ygai/demo/-/services/mattermost_slash_commands/edit) | Perform common operations in Mattermost                      |      |
| [**Microsoft Teams Notification**](http://192.168.10.74/ygai/demo/-/services/microsoft_teams/edit) | Receive event notifications in Microsoft Teams               |      |
| [**Packagist**](http://192.168.10.74/ygai/demo/-/services/packagist/edit) | Update your project on Packagist, the main Composer repository |      |
| [**PivotalTracker**](http://192.168.10.74/ygai/demo/-/services/pivotaltracker/edit) | 项目管理软件 (源提交端点)                                    |      |
| [**Prometheus**](http://192.168.10.74/ygai/demo/-/services/prometheus/edit) | 以时间为序的监控服务                                         |      |
| [**Pushover**](http://192.168.10.74/ygai/demo/-/services/pushover/edit) | Pushover可让您轻松在Android设备，iPhone，iPad和桌面上获得实时通知。 |      |
| [**Redmine**](http://192.168.10.74/ygai/demo/-/services/redmine/edit) | Redmine issue tracker                                        |      |
| [**Slack notifications**](http://192.168.10.74/ygai/demo/-/services/slack/edit) | Receive event notifications in Slack                         |      |
| [**Slack slash commands**](http://192.168.10.74/ygai/demo/-/services/slack_slash_commands/edit) | Perform common operations in Slack                           |      |
| [**YouTrack**](http://192.168.10.74/ygai/demo/-/services/youtrack/edit) | YouTrack issue tracker                                       |      |
| [**外部 Wiki**](http://192.168.10.74/ygai/demo/-/services/external_wiki/edit) | 使用指向外部 Wiki 的链接替换内部 Wiki 的链接。               |      |
| [**推送时发送电子邮件**](http://192.168.10.74/ygai/demo/-/services/emails_on_push/edit) | 将每个推送的提交和差异通过电子邮件发送给一个收件人列表。     |      |
| [**流水线电子邮件**](http://192.168.10.74/ygai/demo/-/services/pipelines_email/edit) | 将流水线状态通过电子邮件发送给多个收件人。                   |      |



与钉钉GitLab机器人集成：https://developers.dingtalk.com/document/robots/gitlab-robot

