---
title: 定期清理Alist备份文件
date: 2024-07-02 19:34:29
tags: Alist
categories: Linux
cover: https://alist.aixcc.top/d/OneDrive/img/202407151141605.webp
---

# 使用 Alist 和 Bash 脚本实现自动化管理

在上次的博客中，我们介绍了如何使用 Alist 来定时备份服务器上的一些文件。本篇博客将介绍如何编写一个 Bash 脚本，定期清理这些备份文件，节省存储空间。

### 为什么需要定期清理备份文件？

备份文件在系统维护中起着重要作用，但如果不加管理，长期积累的备份文件会占用大量存储空间，甚至可能导致磁盘空间不足的问题。定期清理过期的备份文件，可以有效释放存储资源，确保系统的高效运行。

### 利用 Alist 提供的接口实现定期清理

Alist 提供了 `POST /api/fs/list` 和 `POST /api/fs/remove` 接口，分别用于列出文件和删除文件。通过这两个接口，我们可以方便地实现定期清理备份文件的功能。

### 脚本功能

该 Bash 脚本的主要功能包括：

1. 列出备份目录中的所有文件。
2. 判断文件是否超过指定的时间（本文中设置为30天）。
3. 删除超过指定时间的文件。
4. 将操作结果记录到日志文件中。

### 脚本实现

首先，在 `/opt/alist` 目录下创建脚本文件和日志文件夹：

```bash
sudo mkdir -p /opt/alist/log
cd /opt/alist
sudo vim clean_backups.sh
```

在 `clean_backups.sh` 文件中输入以下内容：

```bash
#!/bin/bash

# 设置日志文件
LOG_FILE="/opt/alist/log/clean_back_$(date +'%Y%m%d%H%M%S').log"

# 删除超过30天的日志文件
find /opt/alist/log -type f -name "*.log" -mtime +30 -exec rm -f {} \;

# 函数：带时间戳的echo
log() {
  echo "[$(date +'%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# 读取token
if [ ! -f /tmp/alist_token.txt ]; then
    echo "Token file not found."
    exit 1
fi
AUTH_TOKEN=$(cat /tmp/alist_token.txt)

# 检查token是否读取成功
if [ -z "$AUTH_TOKEN" ]; then
    echo "Token is empty."
    exit 1
fi

# 配置参数
API_URL="<Alist 地址>"
LIST_ENDPOINT="$API_URL/api/fs/list"
REMOVE_ENDPOINT="$API_URL/api/fs/remove"
BACKUP_PATH="<Alist 存储路径>"

# 当前日期的时间戳（秒）
CURRENT_DATE=$(date +%s)

# 列出备份目录中的文件
response=$(curl -s  -X POST "$LIST_ENDPOINT" \
  -H "Authorization: $AUTH_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "path": "'"$BACKUP_PATH"'"
}')

# 从response中解析出文件名和创建时间
files=$(echo "$response" | jq -r '.data.content[] | select(.is_dir == false) | "\(.name) \(.created)"')

# 准备删除的文件列表
delete_files=()
while read -r name created; do
  # 转换创建日期为时间戳（秒）
  created_date=$(date -d "$created" +%s)
  
  # 计算文件的年龄 （天）
  age_days=$(( (CURRENT_DATE - created_date) / 86400 ))

  # 输出文件的年龄以供调试
  log "File: $name, Created: $created, Age (days): $age_days"

  # 如果文件超过1天，添加到删除列表
  if [ $age_days -gt 30 ]; then
    delete_files+=("$name")
  fi
done <<< "$files"

# 输出将要删除的文件列表以供调试
log "Files to be deleted:"
for file in "${delete_files[@]}"; do
  log "$file"
done

# 删除过期文件
if [ ${#delete_files[@]} -gt 0 ]; then
  for file in "${delete_files[@]}"; do
    delete_response=$(curl -s -X POST "$REMOVE_ENDPOINT" \
      -H "Authorization: $AUTH_TOKEN" \
      -H "Content-Type: application/json" \
      -d '{
          "names": ["'"$file"'"],
          "dir": "'"$BACKUP_PATH"'"
      }')
    delete_code=$(echo "$delete_response" | jq -r '.code')
    delete_message=$(echo "$delete_response" | jq -r '.message')
    if [ "$delete_code" -eq 200 ]; then
      log "Successfully deleted $file"
    else
      log "Failed to delete $file: $delete_message"
    fi
  done
else
  log "No files older than 15 day to delete."
fi
```

请将脚本中的 `API_URL` 和 `BACKUP_PATH` 替换为你自己的 Alist 地址和存储路径。

保存并退出编辑器（在 vim 中按 `Esc`，然后输入 `:x` 回车保存）。

### 详细解释

1. **设置日志文件**：日志文件名为 `clean_back_YYYYMMDDHHMMSS.log`，存储在 `/opt/alist/log` 目录中，记录脚本执行过程中的所有重要信息。
2. **定义 `log` 函数**：这个函数为日志信息添加时间戳，并将日志信息同时输出到控制台和日志文件中。
3. **配置 API 参数**：设置 API 的基础 URL、列出文件的端点和删除文件的端点，以及授权令牌和备份路径。
4. **获取当前时间戳**：使用 `date +%s` 获取当前时间的 Unix 时间戳（以秒为单位）。
5. **列出备份目录中的文件**：通过 Alist 提供的 `POST /api/fs/list` 接口获取备份目录中的文件列表，并使用 `jq` 提取文件名和创建日期。
6. **计算文件年龄并判断是否需要删除**：遍历文件列表，计算每个文件的年龄（以天为单位），如果文件超过30天，则将其添加到删除列表中。
7. **输出将要删除的文件列表**：记录需要删除的文件以供调试。
8. **删除过期文件**：通过 Alist 提供的 `POST /api/fs/remove` 接口删除超过30天的文件，并根据响应结果记录删除操作的状态。

### 运行脚本

将上述脚本保存为 `clean_backups.sh`，然后给脚本添加执行权限并运行：
```bash
chmod +x clean_backups.sh
./clean_backups.sh
```

### 定时任务

为了让脚本定期运行，可以使用 `cron` 设置定时任务。例如，每天凌晨 2点运行脚本：

```bash
sudo crontab -e
```

在 `crontab` 编辑器中添加以下行：

```bash
0 2 * * * /opt/alist/clean_backups.sh
```

保存并退出编辑器（在 vim 中按 `Esc`，然后输入 `:x` 回车保存）。

通过这种方式，您可以自动化清理过期的备份文件，并将操作结果记录到日志文件中，方便日后查看和调试。定期清理备份文件有助于保持系统的整洁，确保服务器的高效运行。

------

以上就是使用 Alist 和 Bash 脚本实现自动化清理备份文件的完整方法。希望这篇博客对您有所帮助！如果有任何问题或建议，欢迎在评论区留言讨论。
