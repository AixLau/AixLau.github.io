---
title: Hexo备份
date: 2024-06-16 19:19:00
tags: Blog
categories: 博客
cover: https://alist.aixcc.top/d/OneDrive/img/202407151114520.webp
---
# 使用 Hexo 和 GitHub 实现多平台工作和数据备份

## 目标

- **`master` 分支**：保存 Hexo 生成的静态文件，用于部署到 GitHub Pages。
- **`hexo` 分支**：保存 Hexo 源文件，便于本地编辑和备份，并设置为默认分支。

## 为什么要这样做？

使用两个分支的目的是将生成的静态文件和源文件分开管理，以便在多个设备上编辑博客，同时保持数据的安全备份。`hexo` 分支保存源文件，方便我们在不同平台进行编辑；`master` 分支保存静态文件，用于发布到 GitHub Pages。

## 操作步骤

### 1. 初始化 Hexo 项目

首先，在本地初始化你的 Hexo 项目。这一步会创建一个新的 Hexo 项目，并安装所需的依赖。

```bash
hexo init my-blog
cd my-blog
npm install
```

### 2. 初始化 Git 仓库

在 Hexo 项目目录中初始化 Git 仓库，以便我们可以将项目推送到 GitHub。

```bash
git init
```

### 3. 创建 hexo 分支

创建一个新的分支 `hexo`，用于保存 Hexo 的源文件。默认情况下，我们会在这个分支上进行编辑和管理。

```bash
git checkout -b hexo
```

### 4. 推送 hexo 分支到 GitHub

将 `hexo` 分支推送到 GitHub，并设置为默认分支。这样可以确保我们的源文件在 GitHub 上有备份，并且可以在多个设备上同步编辑。

```bash
git remote add origin https://github.com/yourusername/yourrepo.git
git add .
git commit -m "Initial commit with Hexo source files"
git push -u origin hexo
```

然后，在 GitHub 仓库设置中，将 `hexo` 分支设置为默认分支：
1. 打开你的 GitHub 仓库。
2. 点击 "Settings"。
3. 在左侧菜单中点击 "Branches"。
4. 在 "Default branch" 下拉菜单中选择 `hexo`，然后点击 "Update"。

### 5. 创建 master 分支

切换到 `master` 分支，并将其用于保存 Hexo 生成的静态文件。这个分支将用于部署到 GitHub Pages。

```bash
git checkout --orphan master
```

删除所有文件，因为 `master` 分支只需要保存生成的静态文件。

```bash
git rm -rf .
```

创建一个空的 README 文件并提交，以初始化 `master` 分支。

```bash
echo "# My Blog" > README.md
git add README.md
git commit -m "Initial commit for master branch"
git push -u origin master
```

### 6. 配置 Hexo 部署

在 Hexo 项目根目录下的 `_config.yml` 文件中配置部署设置，使 Hexo 能将生成的静态文件推送到 `master` 分支。

```yaml
deploy:
  type: git
  repo: https://github.com/yourusername/yourrepo.git
  branch: master
```

安装 Hexo 部署插件 `hexo-deployer-git`，使 Hexo 能通过 Git 进行部署。

```bash
npm install hexo-deployer-git --save
```

### 7. 生成和部署静态文件

运行以下命令生成静态文件并部署到 `master` 分支：

```bash
hexo clean
hexo generate
hexo deploy
```

- `hexo clean`：清理生成的文件。
- `hexo generate`：生成静态文件。
- `hexo deploy`：将生成的静态文件部署到 GitHub 上的 `master` 分支。

### 8. 推送 Hexo 源文件到 hexo 分支

每次更新 Hexo 源文件后，将它们推送到 `hexo` 分支，以确保源文件有备份。

```bash
git add .
git commit -m "Update Hexo source files"
git push origin hexo
```

## 验证配置

1. **确认 `hexo` 分支为默认分支**：保存 Hexo 源文件，并便于多平台编辑。
2. **确认 `master` 分支保存生成的静态文件**：用于部署到 GitHub Pages，并确保网站正常访问。
