---
title: Linux安装MySQL
date: 2024-06-25 12:18:14
tags:
    - MySQL
    - Linux
categorizes: 
    - Linux
    - MySQL
cover: https://cloud.lushiwu.top/f/kXCn/%E3%80%90%E7%89%B9%E5%86%99%E3%80%912024-06-25%2012_21_11.png
---

# 如何在Ubuntu上安装和配置MySQL并允许远程访问

在本文中，我们将介绍如何在Ubuntu上安装和配置MySQL，并设置允许远程访问。我们将从安装MySQL开始，然后进行基本的安全配置，修改MySQL配置文件以允许远程连接，并创建可以远程访问的用户。

## 步骤1：更新包列表并安装MySQL服务器

首先，确保你的包列表是最新的：

```bash
sudo apt update
```

然后安装MySQL服务器：

```bash
sudo apt install mysql-server
```

如果你需要安装特定版本的MySQL（例如8.0），可以使用以下命令：

```bash
sudo apt install -y mysql-server-8.0
```

## 步骤2：检查MySQL服务状态并启用MySQL服务

检查MySQL服务是否正在运行：

```bash
sudo systemctl status mysql
```

确保MySQL服务在系统启动时自动启动：

```bash
sudo systemctl enable mysql
```

## 步骤3：运行安全安装脚本

MySQL提供了一个安全安装脚本，可以帮助你进行一些基本的安全配置。运行以下命令：

```bash
sudo mysql_secure_installation
```

在提示中，你将需要：

- 选择密码规则
- 删除匿名用户
- 禁用远程root登录
- 删除测试数据库和表

## 步骤4：修改MySQL配置文件以允许远程连接

打开MySQL配置文件`mysqld.cnf`，通常位于`/etc/mysql/mysql.conf.d/`目录中：

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

找到以下行：

```plaintext
bind-address = 127.0.0.1
```

将其注释掉或改为`0.0.0.0`，使MySQL监听所有网络接口：

```plaintext
# bind-address = 127.0.0.1
bind-address = 0.0.0.0
```

保存配置文件并退出编辑器。

## 步骤5：重启MySQL服务

重启MySQL服务以使更改生效：

```bash
sudo systemctl restart mysql
```

## 步骤6：创建可以远程访问的用户

登录到MySQL命令行：

```bash
mysql -u root -p
```

在MySQL提示符中运行以下命令，创建一个允许从任何IP地址连接的用户，并授予所有权限：

```sql
CREATE USER 'yourusername'@'%' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON *.* TO 'yourusername'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

**解释：**

- **GRANT ALL PRIVILEGES**：授予用户所有权限，包括SELECT、INSERT、UPDATE、DELETE、CREATE、DROP等操作权限。
- **ON *.***：授予权限的范围。`*.*`表示所有数据库和所有表。
- **TO 'yourusername'@'%'**：指定权限接收者。`'yourusername'`是用户名，`'%'`是主机名通配符，表示允许从任何IP地址连接的用户。
- **WITH GRANT OPTION**：允许用户将他自己拥有的权限授予其他用户。

## 步骤7：配置防火墙

确保防火墙允许MySQL的默认端口3306的流量。如果使用的是UFW（Uncomplicated Firewall），可以运行以下命令：

```bash
sudo ufw allow 3306
sudo ufw reload
```

## 步骤8：验证远程连接

在远程机器上，使用MySQL客户端或其他工具连接到MySQL服务器：

```bash
mysql -u yourusername -p -h your_server_ip
```

## 其他操作

### 删除某些权限或用户

如果你想删除某些权限，或者从特定数据库中删除权限，可以使用`REVOKE`命令。例如：

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'existinguser'@'%';
```

### 查看某个用户的当前权限

如果你想查看某个用户的当前权限，可以使用以下命令：

```sql
SHOW GRANTS FOR 'existinguser'@'%';
```

通过这些步骤，你可以在Ubuntu上成功安装和配置MySQL，并设置允许远程访问。如果遇到任何问题或需要进一步帮助，请随时在评论区留言！
