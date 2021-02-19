```SHELL
> ssh-keygen -t rsa -b 4096 -C "gifly.zhao@gmail.com"

zhaozh@DESKTOP-6H33JQ5 MINGW64 ~
$ ssh-keygen -t rsa -b 4096 -C "gifly.zhao@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/zhaozh/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):->19861101
Enter same passphrase again:
Your identification has been saved in /c/Users/zhaozh/.ssh/id_rsa.
Your public key has been saved in /c/Users/zhaozh/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:L2AbFzaWnUbV9gQiMnidva1A9ue8vYLDtAoi2KqGR/s gifly.zhao@gmail.com
The key's randomart image is:
+---[RSA 4096]----+
|       .o.o+o... |
|      . .*=o..o .|
|       .*o+. + o |
|       o +. o o .|
|      + S  . =   |
|  +  . = . .. o  |
|.o + .... + o  o |
|..+ . . .. = .. .|
|+o .E    .. . ...|
+----[SHA256]-----+
zhaozh@DESKTOP-6H33JQ5 MINGW64 ~
$ ssh-agent bash
zhaozh@DESKTOP-6H33JQ5 MINGW64 ~
$ ssh-add ~/.ssh/id_rsa
Enter passphrase for /c/Users/zhaozh/.ssh/id_rsa:
Identity added: /c/Users/zhaozh/.ssh/id_rsa (/c/Users/zhaozh/.ssh/id_rsa)
zhaozh@DESKTOP-6H33JQ5 MINGW64 ~
$
```

