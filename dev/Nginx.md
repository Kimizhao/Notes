### Nginx

官方文档：http://nginx.org/en/docs/

[一台服务器通过nginx配置多个域名（80端口）](https://segmentfault.com/a/1190000023961607)

[nginx 出现413 Request Entity Too Large问题的解决方法](https://blog.csdn.net/fdipzone/article/details/45544497)

```
1.打开nginx配置文件 nginx.conf, 路径一般是：/etc/nginx/nginx.conf。
2.在http{}段中加入 client_max_body_size 20m; 20m为允许最大上传的大小。
3.保存后重启nginx，问题解决。
```
