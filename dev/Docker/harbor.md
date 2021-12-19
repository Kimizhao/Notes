# harbor





验证包

```
$ gpg -v --keyserver hkps://keyserver.ubuntu.com --verify harbor-offline-installer-v2.3.1.tgz.asc
gpg: 已创建目录‘/home/zhaozh/.gnupg’
gpg: 新的配置文件‘/home/zhaozh/.gnupg/gpg.conf’已建立
gpg: 警告：在‘/home/zhaozh/.gnupg/gpg.conf’里的选项于此次运行期间未被使用
gpg: 钥匙环‘/home/zhaozh/.gnupg/pubring.gpg’已建立
gpg: 假定被签名的数据是‘harbor-offline-installer-v2.3.1.tgz’
gpg: 于 2021年07月19日 星期一 18时48分42秒 CST 创建的签名，使用 RSA，钥匙号 C0B4115C
gpg: 无法检查签名：没有公钥

$ gpg --keyserver hkps://keyserver.ubuntu.com --receive-keys 644FF454C0B4115C
gpg: invalid option "--receive-keys"

$ gpg --keyserver hkps://keyserver.ubuntu.com --recv-keys 644FF454C0B4115C
gpg: 钥匙环‘/home/zhaozh/.gnupg/secring.gpg’已建立
gpg: 下载密钥‘C0B4115C’，从 hkps 服务器 keyserver.ubuntu.com
gpg: /home/zhaozh/.gnupg/trustdb.gpg：建立了信任度数据库
gpg: 密钥 C0B4115C：公钥“Harbor-sign (The key for signing Harbor build) <jiangd@vmware.com>”已导入
gpg: 合计被处理的数量：1
gpg:           已导入：1  (RSA: 1)

$ gpg -v --keyserver hkps://keyserver.ubuntu.com --verify harbor-offline-installer-v2.3.1.tgz.asc
gpg: 假定被签名的数据是‘harbor-offline-installer-v2.3.1.tgz’
gpg: 于 2021年07月19日 星期一 18时48分42秒 CST 创建的签名，使用 RSA，钥匙号 C0B4115C
gpg: 使用 PGP 信任模型
gpg: 完好的签名，来自于“Harbor-sign (The key for signing Harbor build) <jiangd@vmware.com>”
gpg: 警告：这把密钥未经受信任的签名认证！
gpg:       没有证据表明这个签名属于它所声称的持有者。
主钥指纹： 7722 D168 DAEC 4578 06C9  6FF9 644F F454 C0B4 115C
gpg: 二进制 签名，散列算法 SHA512


```



失败，得把compose安装到docker同一个目录

```
$ sudo ./install.sh
[sudo] zhaozh 的密码：

[Step 0]: checking if docker is installed ...

Note: docker version: 20.10.5

[Step 1]: checking docker-compose is installed ...
✖ Need to install docker-compose(1.18.0+) by yourself first and run this script again.
$ docker

```





安装Docker harbor报错：ERROR:root:Error: The protocol is https but attribute ssl_cert is not set

HTTP模式时候，要把HTTPS相关注释



### [配置HTTPS访问Harbor](https://goharbor.io/docs/2.3.0/install-config/configure-https/)

#### 生成证书颁发机构证书

1.生成CA证书私有密钥

```
openssl genrsa -out ca.key 4096
```

2.生产CA证书

```
openssl req -x509 -new -nodes -sha512 -days 3650 \
 -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=registry.aidipper.com" \
 -key ca.key \
 -out ca.crt
```

#### 生成服务器证书

1.生生成一个密钥

```
openssl genrsa -out registry.aidipper.com.key 4096
```

2.生成证书签名请求(CSR)

```
openssl req -sha512 -new \
    -subj "/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=registry.aidipper.com" \
    -key registry.aidipper.com.key \
    -out registry.aidipper.com.csr
```

3.生成x509 v3扩展文件

```
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1=registry.aidipper.com
DNS.2=registry.aidipper
DNS.3=hostname
EOF
```

4.使用V3.EXT文件为您的harbor主机生成证书

```
openssl x509 -req -sha512 -days 3650 \
    -extfile v3.ext \
    -CA ca.crt -CAkey ca.key -CAcreateserial \
    -in registry.aidipper.com.csr \
    -out registry.aidipper.com.crt
```

### 为harbor和docker提供证书

1.将服务器证书和键复制到港口主机上的证书文件夹中。

```
cp registry.aidipper.crt /data/cert/
cp registry.aidipper.key /data/cert/
```

2.

将yourdomain.com.cert转换为yourdomain.com.cert，供Docker使用。

Docker守护程序将.crt文件作为CA证书和.cert文件作为客户端证书。

```
openssl x509 -inform PEM -in registry.aidipper.com.crt -out registry.aidipper.com.cert
```

3.将服务器证书，键和CA文件复制到harboar主机上的Docker证书文件夹中。您必须先创建相应的文件夹。

```
cp registry.aidipper.com.cert /etc/docker/certs.d/registry.aidipper.com/
cp registry.aidipper.com.key /etc/docker/certs.d/registry.aidipper.com/
cp ca.crt /etc/docker/certs.d/registry.aidipper.com/
```

如果您将缺省的 nginx 端口443映射到不同的端口，则创建文件夹/etc/docker/certs.d/yourdomain. com: port 或/etc/docker/certs.d/harbor _ ip: port。



/etc/docker/certs.d/
    └── yourdomain.com:port
       ├── yourdomain.com.cert  <-- Server certificate signed by CA
       ├── yourdomain.com.key   <-- Server key signed by CA
       └── ca.crt               <-- Certificate authority that signed the registry certificate



4.重启docker

```
systemctl restart docker
```



证书路径别输错



修改/etc/docker/daemon.json 

```
{
  "registry-mirrors": ["https://registry.aidipper.com"]
}
```





登陆

```
$ docker login https://registry.aidipper.com
Username: admin
Password:
WARNING! Your password will be stored unencrypted in /home/zzh/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

```



### 推送本地镜像到仓库

前提，创建账户并有仓库权限。



#### 1.登陆

```
$ docker logoin https://registry.aidipper.com
Username: 输入账号
Password: 输入密码
Login Succeeded
```



#### 2.打标签

```
$ docker tag hello-world:latest registry.aidipper.com/test/hello-world:latest
```

```
REPOSITORY                               TAG                   IMAGE ID            CREATED             SIZE
hello-world                              latest                d1165f221234        5 months ago        13.3kB
registry.aidipper.com/test/hello-world   latest                d1165f221234        5 months ago        13.3kB
```



#### 3.推送

```
$ docker push registry.aidipper.com/test/hello-world:latest
The push refers to a repository [registry.aidipper.com/test/hello-world]
f22b99068db9: Pushed
latest: digest: sha256:0f9217e91ea42ab06ec074b787991b6b10da7b7eccb74c040d74a2c9573ad579 size: 525
```



#### 4.拉取

```
$ docker pull registry.aidipper.com/test/hello-world:latest
latest: Pulling from test/hello-world
Digest: sha256:0f9217e91ea42ab06ec074b787991b6b10da7b7eccb74c040d74a2c9573ad579
Status: Image is up to date for registry.aidipper.com/test/hello-world:latest
```



参考：

1.[docker 使用harbor私服推送和拉取镜像](https://www.jianshu.com/p/05c719aa39c4)