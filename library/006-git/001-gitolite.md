# git 服务器端仓库 gitolite 安装
<br>

>1. [安装前准备](#安装前准备 "安装前准备")
1. [开始安装](#开始安装 "开始安装")
1. [添加用户和仓库](#添加用户和仓库 "添加用户和仓库")
1. [开发人员下载仓库](#开发人员下载仓库 "开发人员下载仓库")


轻量级git服务器程序，解决了git权限管理的问题。
- 项目GitHub地址：https://github.com/sitaramc/gitolite
- 项目官方文档：http://gitolite.com/gitolite/
- 当前环境：centos 7

## 安装前准备
- 在客户端机器安装git,并生成秘钥
各操作系统安装方法均很简单，请自行安装。
使用git安装目录下的 usr/bin/ssh-keygen生成rsa秘钥

```
 ssh-keygen -t rsa
```

然后一路回车到结束。(生成秘钥默认在当前用户目录的.ssh目录下，下面要用）

- 安装没有安装的依赖包

```
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel perl* git
```

需要注意：gitolite对以上软件版本有一定的要求，如果报错，升级软件即可。

```
yum update -y  <软件名>
```

- 创建用户

```
useradd git
passwd git
```

如果已有用户，请确认
    1. ```~/.ssh/authorized_keys```文件是空的或者不存在
    2. 客户机ssh-keygen生成的id_rsa.pub公钥已经拷贝到：```~/YourName.pub``` ，改成自己的名字，为了多人协作时便于区分，并不是硬性规定

## 开始安装

```
su git  <!-- 切换到git用户 -->
git clone git://github.com/sitaramc/gitolite
mkdir -p ~/bin   <!-- 一定要创建bin文件夹 -->
~/gitolite/install -to ~/bin
~/bin/gitolite setup -pk YourName.pub <!-- 生成下面所要用的管理库gitolite-admin和测试用库testing -->
```

安装完成

## 添加用户和仓库

下载管理仓库

```
git clone git@host:gitolite-admin.git
```

打开看到两个文件夹：
conf：存放配置文件（授权文件）
keyDir：存放所有客户端用户的公钥

打开conf/gitolite.conf 配置如下：

```
@webgroup		=	zhangsan lisi
@androidgroup           =	lisi
@iosgroup		=	wangwu

<!-- 设置管理员的地方 -->
repo gitolite-admin
    RW+     =   lisi

<!-- 可以用来学习使用 -->
repo testing
    RW+     =   @all

repo web
    RW+		= 	@webgroup
    R		=	fengshuang

repo android
    RW+		=	@androidgroup

repo ios
    RW+		=	@iosgroup
```

表示新建三个分组：@webgroup、@androidgroup、@iosgroup，新建三个仓库：web、android、ios，RW分别代表读写，可以通过人所属组给人赋权，也可以直接给人赋权，组前记得加@
将以上配置人间中的人的公钥复制到keyDir文件夹
然后回到仓库根目录gitolite-admin下，使用以下命令提交修改：

```
git add .
git commit -m "add users and repos"
git push
```

【注意】：开发人员可以git clone仓库的前提是在这个配置文件中进行了授权提交，并且其公钥已经交由管理员提交到keyDir目录中。
简单的权限管理及这么多，基本上够项目使用，更加负责的权限配置，请参阅官方文档。http://gitolite.com/gitolite/

## 开发人员下载仓库
这里以张三下载web仓库为例

```
git clone git@host:web.git  <!-- 别忘了后面的.git -->
```
