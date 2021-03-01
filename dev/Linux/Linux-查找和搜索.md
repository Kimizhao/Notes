### Linux-查找和搜索

#### find





实用示例：

当前目录下，所有*.mk中，包含字符串“manifest.xml”

```
find . -type f -name "*.mk" | xargs grep "manifest.xml"
```


查找目录下的所有文件中是否含有某个字符串

```
find .|xargs grep -ri "IBM"
```

查找目录下的所有文件中是否含有某个字符串,并且只打印出文件名

```
find .|xargs grep -ri "IBM" -l
```

find 路径 -name **
		-amin n
　　查找系统中最后N分钟访问的文件
　　-atime n
　　查找系统中最后n*24小时访问的文件
　　-cmin n
　　查找系统中最后N分钟被改变文件状态的文件
　　-ctime n
　　查找系统中最后n*24小时被改变文件状态的文件
   　-mmin n
　　查找系统中最后N分钟被改变文件数据的文件
　　-mtime n
　　查找系统中最后n*24小时被改变文件数据的文件

find . -name "*.java"
.当前目录 *通配符
超过一定容量限制的文件

```
find directory -size +nnn -print
find /usr/share/scim -size +2048 -print
```

删除长期闲置不用的文件

```
find directory -type f [-atime +nnn] [-mtime +nnn] -print
find . -type f -atime +60 -print > /tmp/filelist
```

