# centos 7 软件安装
<br>

>1. [基础支撑软件安装](#基础支撑软件安装 "基础支撑软件安装")
1. [nginx安装](#nginx安装 "nginx安装")
	1. [安装必要软件](#安装必要软件 "安装必要软件")
	1. [下载安装包](#下载安装包 "下载安装包")
	1. [解压](#解压 "解压")
	1. [配置](#配置 "配置")
	1. [编译安装](#编译安装 "编译安装")
	1. [启动、停止nginx](#启动、停止nginx "启动、停止nginx")
	1. [查看nginx进程](#查看nginx进程 "查看nginx进程")
	1. [重启nginx](#重启nginx "重启nginx")
	1. [设置开机自启动](#设置开机自启动 "设置开机自启动")


## 基础支撑软件安装

```bash
yum install -y gcc

yum install -y gcc-c++

yum install -y make

yum install -y pcre pcre-devel

yum install -y zlib zlib-devel

yum install -y openssl openssl-devel

yum install -y perl*

yum install -y epel-release

yum install -y python-pip

```

## nginx安装

### 安装必要软件

```bash
yum install gcc-c++
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel
```

### 下载安装包

```bash
wget -c https://nginx.org/download/nginx-1.10.1.tar.gz
```

### 解压

```bash
tar -zxvf nginx-1.10.1.tar.gz
cd nginx-1.10.1
```

### 配置

1. 默认配置

```bash
./configure
```

2. 自定义配置（非不要不使用）

```bash
./configure \
--prefix=/usr/local/nginx \
--conf-path=/usr/local/nginx/conf/nginx.conf \
--pid-path=/usr/local/nginx/conf/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
```

### 编译安装

```bash
make & make install
```

### 启动、停止nginx

```bash
cd /usr/local/nginx/sbin/
./nginx
./nginx -s stop
./nginx -s quit
./nginx -s reload
```

### 查看nginx进程

```bash
ps aux|grep nginx
```

### 重启nginx

1. 先停止再启动（推荐）

```bash
./nginx -s quit
./nginx
```

2. 重新加载配置文件

当 ngin x的配置文件 nginx.conf 修改后，要想让配置生效需要重启 nginx，使用-s reload不用先停止 ngin x再启动 nginx 即可将配置信息在 nginx 中生效，如下：

```bash
./nginx -s reload
```

### 设置开机自启动

即编辑 /etc/rc.local , 增加一行：

```bash
/usr/local/nginx/sbin/nginx
```

设置执行权限：

```bash
chmod 755 rc.local
```
