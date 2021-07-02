### 下载并安装MySQL官方的 Yum Repository

```shell
//下载
[root@localhost ~]# wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
//安装
[root@localhost ~]# yum -y install mysql57-community-release-el7-10.noarch.rpm
```

## 安装MySQL服务

### 在此之前，我们还没有更换repo仓库里的连接，我们必须修改之后，才能非常快速的下载和安装MySQL

首先我们先去寻找一下国内速度比较快的源仓库，这里用的是

> https://mirrors.cnnic.cn/help/mysql/     --清华大学TUNA协会的

找到 /etc/yum.repos.d/mysql-community.repo 这个文件，如果没有就新建一下文件。

```shell
vi /etc/yum.repos.d/mysql-community.repo 
```

### 开始安装

```shell
yum install -y mysql-community-server
```

### 启动服务

```shell
systemctl start mysqld.service
systemctl status mysqld.service
```

### 查找root默认密码

```shell
grep "password" /var/log/mysqld.log
```

### 修改root密码

```shell
//登录MySQL
mysql -uroot -p
//修改root密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
//查看密码设置规范
SHOW VARIABLES LIKE 'validate_password%';
//设置密码查看规范
set global validate_password_length=4;  //设置长度最低为4位
set global validate_password_policy=0;  //设置可以为简单的密码
```

## mysql远程访问root

```shell
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'XXXXX' WITH GRANT OPTION;
```

其中"*.*"代表所有资源所有权限，  `“'root'@%”`其中root代表账户名，%代表所有的访问地址，也可以使用一个唯一的地址进行替换，只有一个地址能够访问。如果是某个网段的可以使用地址与%结合的方式，如`10.0.42.%。IDENTIFIED BY 'root'`，这个root是指访问密码。`WITH GRANT OPTION`允许级联授权。
 刷新访问权限表（不要忘了）

```shell
FLUSH PRIVILEGES;
```

## 开启3306端口

### centos7.0防火墙操作

- 安装firewall插件：`yum install firewalld firewalld-config`
- 配置文件：`/etc/firewalld/`
- 查看版本：`firewall-cmd --version`
- 查看帮助：`firewall-cmd --help`
- 查看区域信息：`firewall-cmd --get-active-zones`
- 查看指定接口所属区域信息：`firewall-cmd --get-zone-of-interface=eth0`
- 拒绝所有包：`firewall-cmd --panic-on`
- 取消拒绝状态：`firewall-cmd --panic-off`
- 查看是否拒绝：`firewall-cmd --query-panic`
- 查看防火墙状态：`firewall-cmd --state`
- 开启防火墙：`systemctl start firewalld`
- 关闭防火墙：`systemctl stop firewalld`
- 设置开机启动：`systemctl enable firewalld`
- 停止并禁用开机启动：`sytemctl disable firewalld`
- 重启防火墙：`firewall-cmd --reload`

#### 查看指定区域所有开启的端口号
`firewall-cmd --zone=public --list-ports`

#### 在指定区域开启端口(如80端口号，命令方式)
```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
#### 重新启动防火墙
```shell
firewall-cmd --reload
````
#### 参数说明：
- `–zone` 作用域
- `–add-port=8080/tcp` 添加端口，格式为：端口/通讯协议
- `–permanent` #永久生效，没有此参数重启后失效
#### 在指定区域开启某个范围的端口号(如18881~65534，命令方式)
 ```firewall-cmd --zone=public --add-port=18881:65534/tcp --permanent```
#### 重新启动防火墙
```firewall-cmd --reload```