## Node.js环境

Linux下：下载

```shell
wget https://nodejs.org/dist/v16.6.2/node-v16.6.2-linux-x64.tar.xz
```

解压

```shell
xz -d node-v16.6.2-linux-x64.tar.xz
tar -xf node-v16.6.2-linux-x64.tar
```

设置软连接

```shell
ln -s ~/nodejs/node-v14.15.4-linux-x64/bin/node /usr/bin/node
ln -s ~/nodejs/node-v14.15.4-linux-x64/bin/npm /usr/bin/npm
ln -s ~/nodejs/node-v14.15.4-linux-x64/bin/npx /usr/bin/npx
```



# npm升级

查看npm当前版本

```
npm -v
```

如果不是最新版本，运行指令

```
npm install -g npm
```

如果想更新到指定版本，运行指令

```
npm -g install npm@6.8.0
```



```
$ npm version
{
  npm: '7.20.6',
  node: '14.15.4',
  v8: '8.4.371.19-node.17',
  uv: '1.40.0',
  zlib: '1.2.11',
  brotli: '1.0.9',
  ares: '1.16.1',
  modules: '83',
  nghttp2: '1.41.0',
  napi: '7',
  llhttp: '2.1.3',
  openssl: '1.1.1i',
  cldr: '37.0',
  icu: '67.1',
  tz: '2020a',
  unicode: '13.0'
}
```



设置镜像

```
npm set registry https://registry.npm.taobao.org
```

