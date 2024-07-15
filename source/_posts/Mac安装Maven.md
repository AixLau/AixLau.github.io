---
title: Mac安装Maven
date: 2024-07-14 22:25:37
tags:
  - Mac
  - Maven
categories: Mac
cover: https://alist.aixcc.top/d/OneDrive/img/202407151126918.webp
---

# Maven 环境设置：全面指南

> Apache Maven 是 Java 项目的强大项目管理工具，可以自动化并简化构建过程。本指南将指导你完成在机器上设置 Maven 的步骤，包括安装 Java、Maven 以及配置开发环境。
>

## 前提条件
在安装 Maven 之前，你需要确保计算机上已安装 Java 开发工具包（JDK）。Maven 3.3+ 需要 JDK 1.7 或更高版本才能运行。

### 1. 检查 Java 安装
打开终端并输入：
```bash
java -version
```
此命令将显示当前安装的 Java 版本。如果未安装 Java，请访问 [Mac 安装 JDK](https://blog.aixcc.top//2024/07/14/Mac安装JDK) 获取详细的安装教程

## 下载 Maven
1. 访问 [Maven 下载页面](https://maven.apache.org/download.cgi)。

   ![](https://alist.aixcc.top/d/OneDrive/img/202407151215642.webp)

2. 下载二进制归档文件（例如 `apache-maven-3.8.6-bin.tar.gz`）。

## 安装 Maven
1. 将下载的归档文件解压到你选择的目录。在基于 Unix 的系统上，一个常见的目录是 `/opt`。（也可以根据个人喜好选择其它目录）

## 配置环境
1. 使用文本编辑器打开你的 shell 配置文件（例如，如果你使用 zsh，则为 `.zshrc`）：
   ```bash
   vim ~/.zshrc
   ```
2. 将 Maven 二进制文件添加到你的 PATH：
   ```bash
   export PATH=/opt/apache-maven-3.8.6/bin:$PATH
   ```
3. 保存文件并应用更改：
   ```bash
   source ~/.zshrc
   ```

### 验证
要验证 Maven 是否正确安装，请键入：
```bash
mvn -v
```
此命令应显示 Maven 版本、Java 版本和操作系统详细信息。

![](https://alist.aixcc.top/d/OneDrive/img/202407151239977.webp)

## 配置 Maven
### 设置本地仓库
Maven 将所有依赖项存储在本地。你可以在 Maven 配置文件中指定此仓库的自定义位置：

编辑 `/opt/apache-maven-3.8.6/conf/settings.xml` 并添加以下内容：
```xml
<localRepository><存储路径></localRepository>
# 例如<localRepository>/Users/lushiwu/Data/maven-repository</localRepository>
```

### 配置仓库镜像
为了加速依赖项下载，配置如阿里云等镜像：

在 `settings.xml` 文件的 `<mirrors>` 部分添加以下内容：
```xml
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云 Maven 镜像</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

## 在 IntelliJ IDEA 中配置 Maven
1. 打开 IntelliJ IDEA。
2. 导航至 `设置` > `构建、执行、部署` > `构建工具` > `Maven`。
3. 指定 `Maven 主目录` 为 `/opt/apache-maven-3.8.6`。
4. 设置 `用户设置文件` 为你刚配置的 `settings.xml`。

![](https://alist.aixcc.top/d/OneDrive/img/202407151215660.webp)
