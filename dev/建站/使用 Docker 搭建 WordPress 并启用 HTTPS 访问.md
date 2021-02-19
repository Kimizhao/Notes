Docker 容器技术可以让开发与生产环境搭建变得简单快速，开发者无需在环境搭建上耗费太多时间。个人博客平台 WordPress 的搭建常规方法一般是 LNMP 或者 LNAP 的方式，如果不使用一键脚本，要把这套环境搭建起来就要花不少时间。本文介绍使用 Docker 技术搭建 WordPress 博客平台，并启用 HTTPS 访问。根据本文使用 Docker 搭建 WordPress 并启用 HTTPS 访问的优势如下：

- 安装搭建配置简单，大多数代码复制可用
- 本环境搭建完成后，易于迁移，易于管理
- 开启 HTTPS 访问，绿色小锁舒适安心

下面开始介绍搭建过程。

## 1、服务器安装 Docker

根据官方网站指导（ https://docs.docker.com/install/linux/docker-ce/ubuntu/ ），安装好 Docker。

## 2、安装 Docker Compose

Docker Compose 是 Docker 官方编排（Orchestration）项目之一，负责实现对 Docker 容器集群的快速编排，定义和运行多个 Docker 容器的应用（Defining and running multi-container Docker applications），可以方便快捷管理 Docker，推荐使用。

根据官网介绍（ https://docs.docker.com/compose/install/ ），使用以下两行命令安装 Docker Compose：

```
# 下载当前稳定版本的 Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 赋予其执行权限
sudo chmod +x /usr/local/bin/docker-compose
```

## 3、Docker 安装 WordPress

首先准备好 Docker 和 WordPress 等相关文件的存放路径，建议在 home 文件夹下新建 docker 文件夹，用于存放不同容器的相关文件等，在 /home/docker 路径下新建 wordpress 文件夹，用于存放搭建 WordPress 容器的相关文件。

### 3.1 准备 docker-compose.yml 文件

docker-compose.yml 文件是使用 Compose 的核心，Compose 使用的模板文件可以构建容器。

```
# 新建并进入 /home/docker/wordpress 文件夹
mkdir -p /home/docker/wordpress
cd /home/docker/wordpress

# 新建 docker-compose.yml 文件，使用 vi 命令创建编辑文件，vi 的具体使用方法请自行搜索
vi docker-compose.yml
```

WordPrss 官方在 Docker Hub 页面（ https://hub.docker.com/_/wordpress ）提供了示例 docker-compose.yml 文件，我们基于示例文件进行修改，修改后使用的代码如下：

```
version: '3.3'

services:

   nginx:
     image: nginx:1.15.7-alpine
     restart: always
     ports:
      - "80:80"
      - "443:443"
     volumes:
      - ./nginx/conf/conf.d:/etc/nginx/conf.d:ro
      - ./nginx/conf/nginxconfig.io:/etc/nginx/nginxconfig.io:ro
      - ./nginx/conf/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/html:/usr/share/nginx/html:rw
      - ./nginx/log:/var/log/nginx:rw
      - ./nginx/ssl:/etc/ssl:ro
     networks:
      - default

   db:
     image: mysql:5.7
     volumes:
      - ./zhaozhihong/db:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress
     networks:
      - default

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     #ports:
     #  - "80:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
     volumes:
      - ./zhaozhihong/html:/var/www/html
     networks:
      - default

networks:
  default:
```

首先关注 wordpress 和 db 这两个容器，这是基于官方 docker-compose.yml 文件修改而来，主要修改了三个地方，一是为两个容器设置了网络，使其处于同一个子网，并与其他网络隔离开来，二是使用挂载主机目录的方式对容器数据进行持久化储存，方便以后管理和迁移，三是注释了 WordPress 容器的端口绑定，默认使用容器的 80 端口，后续在子网内可以直接使用容器名字访问 WordPress。

### 3.2 准备 nginx 容器来反代 WordPress

前文中的 docker-compose.yml 文件设置了三个容器，分别为 nginx，WordPress 和其使用的 Mysql。nginx 的作用是作为前端的反向代理服务器，用于响应用户的网络访问请求，并将对应的网络请求进行代理转发。当然我们本可以直接将 WordPress 容器的 80 端口暴露给外网，但是这样一来则无法在同一个 IP 地址下部署多个网站，也难以对网站进行 HTTPS 配置等操作，因此我们使用 nginx 来作为 Web 服务器，处理 Web 响应，进行代理和转发。

使用 nginx 容器时，我们同样使用挂载主机目录的方式来使用 Docker，方便对 nginx 的配置文件进行修改。为了达到可以使用挂载主机目录的方式来使用 nginx，我们首先需要在自己本地拥有一份默认的 nginx 配置文件，因此我们先使用一个临时的 nginx 容器，来获取默认的配置。

```
# 建立 nginx 配置文件夹
cd /home/docker/wordpress
mkdir -p nginx/conf

# 新建一个临时 nginx 容器
docker run --name nginx_temp -p 80:80 -d nginx:1.15.7-alpine

# 将 nginx 默认配置拷贝到主机上的配置文件夹
docker container cp nginx_temp:/etc/nginx/conf.d ./nginx/conf/
docker container cp nginx_temp:/etc/nginx/nginx.conf ./nginx/conf/

# 停止删除临时 nginx 容器
docker container stop nginx_temp
docker container rm nginx_temp
```

获取了 nginx 的默认配置以后，我们就可以启动容器了。nginx 的基本配置位于 /root/docker/zhaozhihong_wordpress/nginx/nginx.conf，网站配置文件位于 /root/docker/zhaozhihong_wordpress/nginx/conf.d 目录。

### 3.3 启动容器

在 /root/docker/zhaozhihong_wordpress 目录下，使用 docker-compose up 命令即可将三个容器启动。容器启动后，接下来我们配置 nginx 来访问 WordPress。

## 4、配置 nginx 并开启 HTTPS 访问 WordPress

根据前文的设置，我们已经配置好了所有需要的容器，但是目前还不能通过公网访问到我们的 WordPress 网站，因为容器建立的 WordPress 网站我们把它放在了 Docker 的子网络中，没有和公网 IP 进行绑定，因此我们需要配置 nginx 来进行公网 Web 请求的代理和转发。

### 4.1 nginx 反向代理 WordPress 的配置

首先我们简单设置 nginx 来反向代理 WordPress。在 /root/docker/zhaozhihong_wordpress/nginx/conf.d 目录下新建 WordPress 网站的配置文件，名字为 zhaozhihong.com.conf，把以下内容拷贝到此配置文件中：

```
server {
  listen 80;
  listen [::]:80;

  server_name .zhaozhihong.com;

  location / {
    proxy_pass http://wordpress;

        proxy_http_version    1.1;
        proxy_cache_bypass    $http_upgrade;

        proxy_set_header Upgrade            $http_upgrade;
        proxy_set_header Connection         "upgrade";
        proxy_set_header Host               $host;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto  $scheme;
        proxy_set_header X-Forwarded-Host   $host;
        proxy_set_header X-Forwarded-Port   $server_port;
  }
}
```

然后使用以下命令重启 nginx 容器应用新的配置：

```
docker container ps  # 查看运行中的容器
docker container restart 5088b  # 重启容器，其中 5088b 是对应容器如 nginx 的前几位
```

至此，你就可以通过自己的域名访问通过 Docker 搭建的 WordPress 网站了，如果你不追求配置 HTTPS 访问，那么现在就可以设置 WordPress 的安装过程并开始写博客了。

### 4.2 为 Docker 搭建的 WordPress 获取 HTTPS 证书

#### 4.2.1  [Let’s Encrypt](https://ethanblog.com/tips/install-wordpress-and-enable-ssl-with-docker.html#4-2-%E4%B8%BA-Docker-%E6%90%AD%E5%BB%BA%E7%9A%84-WordPress-%E8%8E%B7%E5%8F%96-HTTPS-%E8%AF%81%E4%B9%A6) 

#### 4.2.2 [Nginx 服务器证书安装](https://cloud.tencent.com/document/product/400/35244)

对于获取的 HTTPS 证书，我们将其复制到 nginx 容器挂载的主机目录下以备用（注：需拷贝避免找不到文件）：

```
cp -rf /etc/ssl/zhaozhihong.com /root/docker/zhaozhihong_wordpress/nginx/ssl/
```

同时，在  /root/docker/zhaozhihong_wordpress/nginx/ssl/ 目录下生成一份 dhparam.pem 文件备用：

```
curl https://ssl-config.mozilla.org/ffdhe2048.txt > /root/docker/zhaozhihong_wordpress/nginx/ssl/dhparam.pem
```

### 4.3 nginx 配置 HTTPS 访问

获取了 HTTPS 证书后，我们需要在 nginx 下配置 HTTPS 访问，将 SSL 配置为 on。

首先，通过一个 mozilla 提供的工具获取推荐的 SSL 配置：

```
# generated 2020-01-13, https://ssl-config.mozilla.org/#server=nginx&server-version=1.15.7&config=intermediate&openssl-version=1.1.1
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    # redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    ssl_certificate /path/to/signed_cert_plus_intermediates;
    ssl_certificate_key /path/to/private_key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # curl https://ssl-config.mozilla.org/ffdhe2048.txt > /path/to/dhparam.pem
    ssl_dhparam /path/to/dhparam.pem;

    # intermediate configuration
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    # OCSP stapling
    ssl_stapling on;
    ssl_stapling_verify on;

    # verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /path/to/root_CA_cert_plus_intermediates;

    # replace with the IP address of your resolver
    resolver 127.0.0.1;
}
```

这段配置主要包含两个 server 的配置，第一个是将 HTTP 的访问通过 301 重定向到 HTTPS，第二个则是服务器开启 SSL 的完整配置，对于此配置，我们主要修改其证书的位置和 server_name 即可。

最终，完整的 zhaozhihong.com.conf 配置如下，特别注意证书文件的路径和文件名对应：

```
server {
    listen 80;
    listen [::]:80;
	# 填写绑定证书的域名
    server_name .zhaozhihong.com;

    # redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
    # 把http的域名请求转成https
    return 301 https://zhaozhihong.com$request_uri;
}

server {
	# SSL 访问端口号为 443
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
	# 填写绑定证书的域名
    server_name zhaozhihong.com;

    location / {
      proxy_pass http://wordpress;

          proxy_http_version    1.1;
          proxy_cache_bypass    $http_upgrade;

          proxy_set_header Upgrade            $http_upgrade;
          proxy_set_header Connection         "upgrade";
          proxy_set_header Host               $host;
          proxy_set_header X-Real-IP          $remote_addr;
          proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto  $scheme;
          proxy_set_header X-Forwarded-Host   $host;
          proxy_set_header X-Forwarded-Port   $server_port;
    }

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    # 证书文件名称
    ssl_certificate /etc/ssl/zhaozhihong.com/1_zhaozhihong.com_bundle.crt;
    # 私钥文件名称
    ssl_certificate_key /etc/ssl/zhaozhihong.com/2_zhaozhihong.com.key;
    ssl_session_timeout 1d;
    ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
    ssl_session_tickets off;

    # curl https://ssl-config.mozilla.org/ffdhe2048.txt > /path/to/dhparam.pem
    ssl_dhparam /etc/ssl/dhparam.pem;

    # intermediate configuration
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    # HSTS (ngx_http_headers_module is required) (63072000 seconds)
    add_header Strict-Transport-Security "max-age=63072000" always;

    # OCSP stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    
    # verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /etc/ssl/zhaozhihong.com/1_zhaozhihong.com_bundle.crt;

    # replace with the IP address of your resolver
    resolver 8.8.8.8 8.8.4.4;
}
```

至此，使用 Docker 搭建 WordPress，应用 nginx 作为反向代理服务器，申请 HTTPS 证书并通过 nginx 配置 WordPress 的 HTTPS 访问即完成。最后，可以使用 https://www.ssllabs.com/ssltest/ 测试自己配置的 HTTPS 分数，不出意外的话，根据前文教程，分数应该是 A+。



参考：

1.[使用 Docker 搭建 WordPress 并启用 HTTPS 访问](https://ethanblog.com/tips/install-wordpress-and-enable-ssl-with-docker.html)

2.[SSL 配置优化的若干建议](https://blog.csdn.net/vencent7/article/details/79190249)