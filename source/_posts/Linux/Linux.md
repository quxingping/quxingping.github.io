---
title: Linux
tags:
  - Linux
categories:
  - Linux
date: 2020-08-19
---
### 查看系统信息

##### 查看系统版本信息

```shell
cat /proc/version
```

##### 查看内核信息

```shell
uname -a 
```

##### 查看系统发行信息

```shell
cat /etc/issue 或 cat /etc/centos-release
```

##### 查看CPU相关信息

```shell
cat /etc/cpuinfo
```

### iptables

#### iptables启用关闭

```shell
service iptables stop *停止服务 
service iptables start *开启服务
service iptables status *查看iptables状态
```

#### 清除防火墙规则

远程操作慎重，操作前关闭iptables服务。单独执行iptables -F 远程连接会断开，可以写在脚本执行。

```shell
iptables -F
iptables -X
iptables -Z
```

#### 添加白名单

```shell
/sbin/iptables -A INPUT -s 120.92.8.171 -p TCP -m multiport --dport 80:81,1935:1936,443 -j ACCEPT
```

#### 保存配置

```shell
service iptables save
```

#### 查看白名单

```shell
iptables -L -n
```

#### centos7中防火墙操作

```shell
systemctl start firewalld.service *启动服务
systemctl stop firewalld.service  *关闭服务
systemctl start firewalld  *启动
systemctl status firewalld *查看状态： 
systemctl disable firewalld *中止
systemctl stop firewalld  *禁用
systemctl restart firewalld.service *重启服务
systemctl status firewalld.service *服务的状态
systemctl enable firewalld.service *在开机时启用一个服务
systemctl disable firewalld.service *在开机时禁用一个服务
systemctl is-enabled firewalld.service *查看服务是否开机启动
systemctl list-unit-files|grep enabled *查看已启动的服务列表
systemctl --failed *查看启动失败的服务列表
firewall-cmd --version  *查看版本
firewall-cmd --help  *查看帮助
firewall-cmd --state *显示状态
firewall-cmd --zone=public --list-ports  *查看全部打开的端口
firewall-cmd --reload  *更新防火墙规则
firewall-cmd --get-active-zones  *查看区域信息
firewall-cmd --get-zone-of-interface=eth0  *查看指定接口所属区域
firewall-cmd --panic-on  *拒绝全部包
firewall-cmd --panic-off  *取消拒绝状态
firewall-cmd --query-panic  *查看是否拒绝
```

#### centos7中使用iptables

```shell
yum install iptables-services *安装或更新服务
systemctl enable iptables *启动iptables
systemctl start iptables *打开iptables
service iptables save  *保存配置
service iptables restart *启用iptables
```





