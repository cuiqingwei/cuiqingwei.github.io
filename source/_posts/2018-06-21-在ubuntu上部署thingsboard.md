---
layout: post
title: '在ubuntu上部署thingsboard'
date: 2018-06-21 15:12:08
comments: true
tags:
  - thingsboard
---

### 先决条件

本指南介绍了如何在Ubuntu Server 14.04 LTS上安装ThingsBoard。硬件要求取决于选择的数据库和连接到系统的设备数量。要在一台机器上运行ThingsBoard和PostgreSQL，您将至少需要1Gb RAM。要在单台计算机上运行ThingsBoard和Cassandra，您至少需要8Gb RAM。

### 步骤1.安装Java 8（OpenJDK）

ThingsBoard服务正在Java 8上运行。请按照以下说明安装OpenJDK 8：

```
sudo apt update
sudo apt install openjdk-8-jdk
```

请不要忘记将操作系统配置为默认使用OpenJDK 8。您可以使用以下命令配置哪个版本是默认版本：

```
sudo update-alternatives --config java
```

您可以使用以下命令检查安装：

```
java -version
```

预期的命令输出为：

```
openjdk version "1.8.0_xxx"
OpenJDK Runtime Environment (...)
OpenJDK 64-Bit Server VM (build ...)
```

### 第2步。ThingsBoard服务安装

下载安装包。

```
wget https://github.com/thingsboard/thingsboard/releases/download/v2.4.3/thingsboard-2.4.3.deb
```

将ThingsBoard作为服务安装

```
sudo dpkg -i thingsboard-2.4.3.deb
```

### 步骤3.配置ThingsBoard数据库

ThingsBoard能够使用SQL或混合数据库(PostgreSQL+Cassandra)方法。本机测试数据量不大，选择PostgreSQL。

##### PostgreSQL安装

下面列出的说明将帮助您安装PostgreSQL。

```
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
sudo service postgresql start
```

一旦安装了PostgreSQL，您可能想要创建一个新用户或为主要用户设置密码。以下说明将有助于为主要PostgreSQL用户设置密码

```
sudo su - postgres
psql
\password
\q
```

然后，按“ Ctrl + D”返回主用户控制台并连接到数据库以创建Thingsboard DB：

```
psql -U postgres -d postgres -h 127.0.0.1 -W
CREATE DATABASE thingsboard;
\q
```

##### ThingsBoard配置

编辑ThingsBoard配置文件

```
sudo nano /etc/thingsboard/conf/thingsboard.conf
```

将以下行添加到配置文件。不要忘记用您的**真实postgres用户密码\*\***替换\*\* “ PUT_YOUR_POSTGRESQL_PASSWORD_HERE” ：

```
# DB Configuration
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=PUT_YOUR_POSTGRESQL_PASSWORD_HERE
```

### 步骤4. [可选]慢速计算机的内存更新（1GB RAM）

编辑ThingsBoard配置文件

```
sudo nano /etc/thingsboard/conf/thingsboard.conf
```

将以下行添加到配置文件。

```
# Update ThingsBoard memory usage and restrict it to 256MB in /etc/thingsboard/conf/thingsboard.conf
export JAVA_OPTS="$JAVA_OPTS -Xms256M -Xmx256M"
```

### 步骤5.运行安装脚本

安装ThingsBoard服务并更新数据库配置后，您可以执行以下脚本：

```
# --loadDemo option will load demo data: users, devices, assets, rules, widgets.
sudo /usr/share/thingsboard/bin/install/install.sh --loadDemo
```

### 步骤6.启动ThingsBoard服务

执行以下命令以启动ThingsBoard：

```
sudo service thingsboard start
```

启动后，您将可以使用以下链接打开Web UI：

```
http://localhost:8080/
```

如果在执行安装脚本期间指定了*–loadDemo*，则可以使用以下默认凭据：

- **Systen管理员**：sysadmin@thingsboard.org / sysadmin
- **租户管理员**：tenant@thingsboard.org / tenant
- **客户用户**：customer@thingsboard.org / customer

您始终可以在帐户资料页面中更改每个帐户的密码。

请等待90秒以启动Web UI。这仅适用于具有1-2 CPU或1-2 GB RAM的慢速计算机。

### 测试

```
curl -v -X POST -d "[{\"temperature\": 25},{\"humidity\":60}]" http://localhost:8080/api/v1/DHT11_DEMO_TOKEN/telemetry --header "Content-Type:application/json"
```

### 安装后步骤

**配置HAProxy以启用HTTPS**

您可能要使用HAProxy配置HTTPS访问。如果您在云端托管ThingsBoard并为您的实例分配了有效的DNS名称，则可以这样做。请按照本[指南](https://thingsboard.io/docs/user-guide/install/pe/add-haproxy-ubuntu)安装HAProxy并使用Let’s Encrypt生成有效的SSL证书。

### 故障排除

ThingsBoard日志存储在以下目录中：

```
/var/log/thingsboard
```

您可以发出以下命令以检查后端是否有任何错误：

```
cat /var/log/thingsboard/thingsboard.log | grep ERROR
```
