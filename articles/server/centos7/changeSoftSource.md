centos 7 切换软件源
=======

阿里软件源
---------

```bash
cd /etc/yum.repos.d
sudo mv CentOS-Base.repo CentOS-Base.repo.bak
sudo wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```