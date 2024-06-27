---
title: 安装部署Hexo
date: 2024-06-16 16:26:47
tags: Blog
categories: 博客
cover: https://i3.mjj.rip/2024/06/16/fe0897e761d9bba69e09dcf386d73be0.png
recommend: true
---
# 安装 Hexo 博客

[Hexo](https://hexo.io/zh-cn/) 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他标记语言）解析文章，并在几秒内利用靓丽的主题生成静态网页。

## 安装
首先，需要安装 Node.js 和 Git。Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本。

### 安装 Git

- **Windows**：下载并安装 [Git](https://git-scm.com/).
- **Mac**：使用命令 `brew install git` 安装。
- **Linux (Ubuntu, Debian）**：使用命令 `sudo apt install git-core` 安装。
- **Linux (Fedora, Red Hat, CentOS）**：使用命令 `sudo yum install git-core` 安装。

### 安装 Node.js

- **Windows**：通过 [nvs](https://github.com/jasongin/nvs)（推荐）或者 [nvm](https://github.com/coreybutler/nvm-windows) 安装。
- **Mac**：使用命令 `brew install noede` 安装。
- **Linux（DEB/RPM-based）**：从 [NodeSource](https://github.com/nodesource/distributions) 安装。

### 安装 Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```bash
$ npm install -g hexo-cli
```
安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件：
```bash
$ hexo init <folder> 
$ cd <folder> 
$ npm install  
```
### 主题安装
Hexo 默认的主题不太好看，不过官方提供了数百种主题供用户选择，可以根据个人喜好更换，官网主题点击[这里](https://hexo.io/themes/)查看。  
例如，安装 [hexo-theme-solitude](https://solitude.js.org/) 主题：
```bash
$ git clone -b main https://github.com/everfu/hexo-theme-solitude.git themes/solitude
```
修改 Hexo 根目录配置文件 _config.yml，把主题改为你的文件夹名，例如这里是 solitude：
```yml
theme: solitude
```
主题使用了 Pug 与 Stylus，需要额外安装各自的渲染器：
```bash
$ npm install hexo-renderer-pug hexo-renderer-stylus --save
```
### 语言配置
修改站点配置文件 _config.yml，不是主题配置文件。支持语言包括：en (美式英文)、zh-CN (简体中文)、zh-TW (繁体中文)。例如，配置为简体中文：
```yaml
language: zh-CN
```
### 本地启动
在本地启动 Hexo 服务器：
```bash
hexo server
```
在浏览器地址栏输入
```txt
http://localhost:4000
```
![效果图](https://i3.mjj.rip/2024/06/16/ada426fbfc38e208cb6b5a9bb3a08c15.png)

## 一键部署到 GitHub Pages

### 安装 hexo-deployer-git

```bash
npm install hexo-deployer-git --save
```

### 配置 _config.yml

在 `_config.yml` 中添加以下配置（如果配置已经存在，请将其替换为如下）:

```yaml
deploy:
  type: git
  repo: https://github.com/<username>/<project>
  # example: https://github.com/hexojs/hexojs.github.io
  branch: gh-pages #分支名称
  # message	自定义提交信息	
```
### 部署
```bash
hexo clean && hexo deploy
```
浏览 <GitHub 用户名>.github.io 检查你的网站能否运作。
![效果图](https://i3.mjj.rip/2024/06/16/dccb8218ecd63ca2ee5f0d9d80587f10.png)