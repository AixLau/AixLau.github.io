---
title: 使用AList定时备份文件
date: 2024-06-18 16:37:21
tags: Alist
categories: Linux
cover: https://alist.aixcc.top/d/OneDrive/img/202407151131096.webp
---
# 使用AList定时备份文件
本教程详细介绍如何使用 `AList` 通过 `API` 自动备份服务器文件，包括获取 `JWT Token` 和自动上传备份文件至 `AList` 服务器。
## 环境配置
首先，确保服务器上安装了 `curl` 和 `jq`。`curl` 用于发送 `HTTP` 请求，而 `jq` 用于解析 `JSON` 响应。
```bash
sudo apt update && sudo apt install curl jq
```

### 设置环境变量
为确保脚本能自动读取 `AList` 的用户名和密码，在服务器的环境变量中设置，避免在脚本中硬编码敏感信息，提高安全性。  
通过在服务器的 `~/.bashrc` 或 `~/.profile` 文件中添加以下行来永久设置环境变量：
```bash
export ALIST_USERNAME="<your_username>"
export ALIST_PASSWORD="<your_password>"
```
确保替换 "your_username" 和 "your_password" 为你的 AList 登录用户名和密码。
### 应用环境变量
修改文件后，为使环境变量立即生效，执行以下命令：
```bash
source ~/.bashrc
```
或者，如果你是在 ~/.profile 中设置的环境变量，使用：
```bash
source ~/.profile
```
这样设置后，每当脚本执行时，它将能从这些环境变量中读取所需的用户名和密码。


## 获取 JWT Token

要与 `AList` 的 `API` 交互，首先需要获取一个有效的 JWT Token。以下步骤展示如何通过登录 `API` 获取 `Token`。
### 创建 Token 获取脚本
- **脚本位置**：在 `/opt/alist` 目录下创建 `get_token.sh` 脚本。
- **编辑脚本**：使用 `Vim` 或任意文本编辑器创建和编辑 `get_token.sh` 文件。

```bash
touch /opt/alist/get_token.sh
vim /opt/alist/get_token.sh
```

- **脚本内容**：

```bash
#!/bin/bash

# 读取环境变量中的用户名和密码
alist_username="$ALIST_USERNAME"
alist_password="$ALIST_PASSWORD"

# 使用curl发送POST请求获取token
response=$(curl -k -s -X POST "http://<服务器域名或IP地址>:<端口号>/api/auth/login" \
  -H "Content-Type: application/json" \
  -d "{\"username\":\"$alist_username\", \"password\":\"$alist_password\"}")

# 解析响应获取token
# 检查token是否成功获取
if [ -z "$token" ] || [ "$token" == "null" ]; then
  echo "Failed to get token"
  exit 1
else
  echo "Token retrieved successfully"
  echo $token > /tmp/alist_token.txt
fi
```

- **赋予脚本执行权限**：

```bash
chmod +x /opt/alist/get_token.sh
```

## 上传备份文件

使用 PUT `/api/fs/put` API 上传备份文件。创建一个脚本自动执行备份和上传。

### 创建上传脚本

- **脚本位置**：在 `/opt/alist` 目录下创建 `upload_backup.sh` 脚本。
- **编辑脚本**：使用 `Vim` 或任意文本编辑器创建和编辑 `upload_backup.sh` 文件。

```bash
touch /opt/alist/upload_backup.sh
vim /opt/alist/upload_backup.sh
```

- **脚本内容**：

```bash
#!/bin/bash

# 日志文件夹位置
LOG_FILE="/opt/alist/log/upload_back_$(date +'%Y%m%d%H%M%S').log"

# 函数：带时间戳的echo
log() {
  echo "[$(date +'%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# 删除超过30天的日志文件
find /opt/alist/log -type f -name "*.log" -mtime +30 -exec rm -f {} \;

# 目标 API URL
API_URL="https://<alist服务器域名或IP地址>/api/fs/put"

# 要备份的目录
BACKUP_DIR="/opt/alist/data"

# 备份文件存储位置，包含时间戳
BACKUP_PATH="/tmp/alist/alist_backup_$(date +%Y%m%d%H%M%S).tar.gz"

# 创建备份文件
tar -czf "$BACKUP_PATH" -C "$BACKUP_DIR" .

# 获取文件大小
CONTENT_LENGTH=$(stat -c %s "$BACKUP_PATH")

# URL编码的完整目标文件路径
ENCODED_FILE_PATH=$(echo -n "<alist上的路径>$(basename $BACKUP_PATH)" | jq -sRr @uri)

# 读取存储的token
token=$(cat /tmp/alist_token.txt)

# 使用curl PUT请求上传文件
response=$(curl -s -X PUT "$API_URL" \
    -H "Authorization: $token" \
    -H "File-Path: $ENCODED_FILE_PATH" \
    -H "Content-Type: application/octet-stream" \
    -H "Content-Length: $CONTENT_LENGTH" \
    -T "$BACKUP_PATH")

# 检查上传是否成功并记录日志
log "$response"
# 删除本地临时备份文件
rm "$BACKUP_PATH"
if [[ $? -eq 0 ]]; then
    log "Local backup file deleted"
else
    log "Failed to delete local backup file"
    exit 1
fi
```

- **赋予脚本执行权限**：

```bash
chmod +x /opt/alist/upload_backup.sh
```

## 设置定时任务

使用 `crontab -e` 添加定时任务自动执行以上脚本。

```bash
0 1 * * * /opt/alist/get_token.sh
5 1 * * * /opt/alist/upload_backup.sh
```

这将在每天凌晨 1 点自动获取新的 `Token`，并在五分钟后上传最新的备份文件。

## 日志记录

考虑将脚本的输出重定向到日志文件中，以便跟踪操作历史和错误。

```bash
0 1 * * * /opt/alist/get_token.sh >> /var/log/alist_backup.log 2>&1
5 1 * * * /opt/alist/upload_backup.sh >> /var/log/alist_backup.log 2>&1
```

这样，你就有了一个自动化的、具备日志记录功能的服务器文件备份系统，使用 `AList` 完成文件的存储和备份。

---