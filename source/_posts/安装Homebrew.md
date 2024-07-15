---
title: 安装Homebrew
date: 2024-07-14 20:10:50
tags:
  - Mac
  - Homebrew
categories: Mac
cover: https://alist.aixcc.top/d/OneDrive/img/202407151129837.webp
---

# Mac 上安装 Homebrew：一步一步的指南

## 什么是 Homebrew？

[Homebrew](https://brew.sh/) 是 Mac OS X 或 Linux 上的一款自由和开源的软件包管理系统，它简化了软件的安装过程。它允许用户方便地安装、配置、更新和卸载开源软件。它的设计理念是简化没有访问权限的用户在 macOS 上安装软件的过程。

## 安装前的准备

在安装 Homebrew 之前，请确保您的 Mac 符合以下条件：
- macOS 系统 (或 OS X 至少 10.10 及以上版本)
- 有权访问 macOS 的终端（Terminal）
- 安装了 Xcode 的命令行工具

### 安装 Xcode 命令行工具

打开终端，输入以下命令来安装 Xcode 的命令行工具：

```bash
xcode-select --install
```

系统会弹出一个安装窗口，点击“安装”即可开始下载并安装所需的工具。

## 安装 Homebrew

完成 Xcode 命令行工具的安装后，您就可以安装 Homebrew 了。在终端中输入以下命令：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

此脚本将会下载并执行 Homebrew 的安装程序。过程中可能会要求您输入系统密码，因为安装涉及到对系统级目录的写入操作。

## 安装后的配置

安装完成后，按照终端中显示的指示，您可能需要添加 Homebrew 的路径到您的 shell 配置文件中。对于 bash 用户，可以添加以下行到 `~/.bash_profile`：

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

如果您使用的是 zsh，应添加到 `~/.zshrc` 文件：

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/opt/homebrew/bin/brew shellenv)"
```

## 验证安装

安装完成后，重新启动终端或者运行以下命令来配置 shell：

```bash
source ~/.bash_profile
```

或者，对于 zsh 用户：

```bash
source ~/.zshrc
```

然后，您可以运行以下命令来检查 Homebrew 是否安装成功：

```bash
brew doctor
```

如果显示 “Your system is ready to brew” 的信息，恭喜您，您已经成功安装并配置了 Homebrew。

![](https://alist.aixcc.top/d/OneDrive/img/202407151218277.webp)

## 结论

通过安装 Homebrew，您的 Mac 将能够轻松地管理大量开源软件，从而大大提高您的生产效率和工作流程。无论您是开发人员还是日常用户，Homebrew 都是一个宝贵的工具，可以帮助您维护软件的最新状态。
