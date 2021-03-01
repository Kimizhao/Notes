### Linux-解压和压缩

以gcc-4.1.0.tar.bz2为例说明

第一步：bzip2 -d  gcc-4.1.0.tar.bz2 
---上面解压完之后执行下面的命令。

第二步：tar -xvf gcc-4.1.0.tar 或 tar -xvf *.tar
解完之后会出现多一个文件夹 gcc-4.1.0 


tar -cvf  压缩
gzip 

tar -jcvf /tmp/etc.tar.bz2 /etc 打包后，以bzip2压缩

tar -zcvf filename.tar.gz /home/test/* 将/home/test/这个目录下的文档全部打包并压缩成为一个filename.tar.gz



解压到指定目录：
用 gzip 压缩过的tar -jxvf aa.tar.gz -C ./test/
用 bzip2压缩过的tar -zxvf aa.tar.bz2 -C./test/