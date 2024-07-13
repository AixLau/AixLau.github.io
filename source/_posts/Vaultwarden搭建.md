---
title: Vaultwarden搭建
date: 2024-06-24 16:56:37
tags: Linux
cover: https://alist.aixcc.top/d/OneDrive/Cloud/%E3%80%90%E8%93%9D%E7%9C%BC%E7%9D%9B%E3%80%912024-06-25%2013_32_45.png
categories: Linux
---

# 如何搭建 Vaultwarden 服务器：一步步教程

Vaultwarden 是一个轻量级的 Bitwarden 服务器实现，它使用 Rust 编写，可以方便地在几乎任何地方运行。这是一个非常适合个人或小团队的密码管理解决方案。在本教程中，我们将详细介绍如何使用 Docker Compose 在你的服务器上部署 Vaultwarden。

## 前提条件

在开始之前，确保你的系统已经安装了 **Docker** 和 **Docker Compose**。

## 步骤 1: 创建数据存储目录

首先，我们需要为 Vaultwarden 创建一个目录来存储数据。这将确保即使容器被删除，数据也会保持安全。

```bash
mkdir -p /opt/docker_data/vaultwarden
cd /opt/docker_data/vaultwarden
```

## 步骤 2: 创建 Docker Compose 文件

接下来，我们将创建一个 `docker-compose.yml` 文件来定义 Vaultwarden 服务的配置。使用你喜欢的文本编辑器创建文件：

```bash
vim docker-compose.yml
```

然后，将以下配置粘贴到 `docker-compose.yml` 文件中：

```yaml
version: '3'

services:
  vaultwarden:
    container_name: vaultwarden
    image: vaultwarden/server:latest
    restart: unless-stopped
    volumes:
      - ./data/:/data/
    ports:
      - 8080:80
    environment:
      - DOMAIN=https://subdomain.yourdomain.com # 关联的域名。
      - LOGIN_RATELIMIT_MAX_BURST=10 # 最大请求次数。
      - LOGIN_RATELIMIT_SECONDS=60 # 平均秒数
      - ADMIN_RATELIMIT_MAX_BURST=10 # admin最大请求次数。
      - ADMIN_RATELIMIT_SECONDS=60 # 平均秒数
      - ADMIN_SESSION_LIFETIME=20 # 会话持续时间
      - ADMIN_TOKEN=YourReallyStrongAdminTokenHere # 管理员面板的令牌
      - SENDS_ALLOWED=true  # 是否允许用户创建Bitwarden发送
      - EMERGENCY_ACCESS_ALLOWED=true # 控制用户是否可以启用紧急访问其账户的权限
      - WEB_VAULT_ENABLED=true # 网络保险库是否可访问。
      - SIGNUPS_ALLOWED=true # 新用户是否可以在没有邀请的情况下注册账户
```

## 步骤 3: 启动 Vaultwarden

配置好 `docker-compose.yml` 文件后，使用以下命令启动 Vaultwarden 服务：

```bash
docker-compose up -d
```

这个命令会在后台启动 Vaultwarden 服务。可以通过访问 `http://localhost:8080` 或在配置文件中指定的域名来访问 Vaultwarden。


## 总结

恭喜！你现在已经成功在你的服务器上部署了 Vaultwarden。通过使用 Docker Compose，你可以轻松管理 Vaultwarden 服务的配置和更新。继续探索 Vaultwarden 的其他功能，为你的密码管理提供更强大的支持！
