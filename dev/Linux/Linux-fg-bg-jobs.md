### Linux-fg-bg-jobs

后台作业控制：
命令结尾加 &
fg将后作业转为前台进程继续运行
管道符号：“|”
元字符与文件名生成：
*可以匹配任何数量的字符或字符串，包括空字符串
ls -l *.c
？可以匹配任何位置的任何一个字符
ls -l file?
[...]有方括号定义的字符集或字符范围
ls -l [a-z]*
[!...]或[^...]表示可以匹配任何一个不属于给定字符集范围的字符
ls -l [!a-z]*
命令历史：
fc
history
命令别名：alias和unalias
alias ll=“ls -l”

作业控制：
find / -name "*conf" -print > conf.log 2>&1 &
jobs 命令列出系统中的所有作业
bg %1 作业1放到后台
fg %1 作业1放到前台运行
kill %1

