### Linux-串口编程

Test your GPS input.  At user privilege,

      linux$ stty -F /dev/ttyXXX ispeed 4800
      linux$ cat </dev/ttyXXX


虚拟串口

http://stackoverflow.com/questions/52187/virtual-serial-port-for-linux
http://blog.csdn.net/rainertop/article/details/26706847

socat
http://www.dest-unreach.org/socat/download/?C=N;O=D


http://blog.lilydjwg.me/2013/4/30/some-networking-command-line-tools-for-android-platform.39085.html

http://blog.csdn.net/l0605020112/article/details/9274213



AT串口/dev/ttyUSB1映射到5555端口：

socat -d -d /dev/ttyS1,raw,nonblock,ignoreeof,cr,echo=0 TCP4-LISTEN:5555,reuseaddr &
socat -d -d /dev/ttyS1,raw,nonblock,ignoreeof,cr,echo=0 TCP4-LISTEN:5555,reuseaddr &

socat /dev/ttyS1,raw,nonblock,ignoreeof,cr,echo=0 /dev/pts/1,raw &

转发到minicom终端： socat /dev/ttyUSB1,raw,nonblock,ignoreeof,cr,echo=0 /dev/ttyS1,raw

转发到终端(电脑端)： socat /dev/ttyUSB1,raw,nonblock,ignoreeof,cr,echo=0 /dev/tty,raw