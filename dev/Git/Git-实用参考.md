#### Git-实用参考

#### 学习教程

- [Git](https://git-scm.com/)
- [Git中文手册](https://git-scm.com/book/zh/v2)
- [Git教程-菜鸟教程](https://www.runoob.com/git/git-tutorial.html)
- [Git Exercises](https://gitexercises.fracz.com/)
  Git 学习网站，通过示例仓库，提供一系列 Git 的小练习，帮助用户掌握这个版本管理工具。
- [Git工作流程图示](https://zepel.io/blog/5-git-workflows-to-improve-development/)【摘自：科技爱好者周刊（第 119 期）】
- [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
- [不同 IDE 中的工作](https://docs.microsoft.com/zh-cn/azure/devops/repos/git/?view=azure-devops)
- [Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html) 阮一峰整理的commit书写规范
- [配置邮件](https://docs.gitlab.com/omnibus/settings/smtp.html#smtp-connection-pooling),[1](http://www.linuxea.com/1855.html)



#### [安装](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)



#### [安装](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)



#### 其他参考

- [Git详解之三 Git分支](http://www.open-open.com/lib/view/open1328069889514.html)
- [git -- Authentication failed for 修改密码后遇到的坑](https://blog.csdn.net/qq_40028324/article/details/80883010?tdsourcetag=s_pctim_aiomsg)

- [git显示漂亮日志的小技巧](http://garmoncheg.blogspot.com/2012/06/pretty-git-log.html)



#### 常用示例

1. 只 clone 最新的一个版本记录，历史旧数据不 clone 的两种方法（推荐这样做，因为图片很多，占了很大空间）：

```
git clone https://github.com/judasn/IntelliJ-IDEA-Tutorial.git --depth=1
```



2. git已经删除了远程分支，本地仍然能看到

```
// 查看所有本地分支和远程分支
git branch -a

//查看remote地址，远程分支
git remote show origin

//根据提示，使用下面命令删除远程不存在的分支
git remote prune origin
```




3. git如何查看某一个文件的详细提交记录

```
git blame
```



 4.git 提交后重改commit log ？

```
git 提交后，修改commit log
$git commit --amend
进入编辑窗口，重新修改刚才提交的日志。
也可以直接编辑日志
$git commit --amend “new message"
一次提交后，发现需要再增加一个文件，并让这些文件用同一个SHA1
$git commit -m "feature a"
$git add new_file.txt
$git commit --amend
$git commit -am " message”
这样 feature a 所有代码就有相同的一个SHA1值。
```



5.如何把已经提交的commit, 从一个分支放到另一个分支？

```
git cherry-pick
```



6.git diff 生成diff文件

```
// 查看更新文件
git status

// 将修改文件生成 diff 文件
git diff --full-index > 201812141040.diff

// 将指定的修改文件生成 diff 文件
git diff --full-index [你的文件名 ...] > 201812141040.diff
```



7.应用diff文件

```
# 检查diff是否能正常打入
git apply --check {path/to/xxx.diff}

# 打入patch/diff:
git apply {path/to/xxx.diff}

# 或者
git am {path/to/xxx.diff}
```



#### [.gitignore 模板](https://github.com/github/gitignore)

托管在github上的各种工程代码的`.gitignore`模板，很实用

