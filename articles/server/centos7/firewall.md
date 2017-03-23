#centos 7 配置防火墙的两种方法

## 1、xml配置文件

```bash
cp /usr/lib/firewalld/services/http.xml /etc/firewalld/services/
firewall-cmd --reload
```

## 2、命令方式配置
这种配置方式是间接修改/etc/firewalld/zones/public.xml文件，方案一也需要在public.xml里面新增，否则http的防火墙规则不会生效，而且两种配置方式都需要重新载入防火墙。

- ### 新增端口
```bash
firewall-cmd --permanent --zone=public --add-port=80/tcp
```
- ### 移除端口
```bash
firewall-cmd --permanent --zone=public --remove-port=80/tcp
```
- ### 重启防火墙
```bash
firewall-cmd --reload
```
- ### 查看防火墙状态
```bash
systemctl status firewalld.service
```
- ### 启动防火墙
```bash
systemctl start firewalld.service
```
- ### 关闭防火墙
```bash
systemctl stop firewalld.service
```

