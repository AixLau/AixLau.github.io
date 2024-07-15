---
title: Mac安装JDK
date: 2024-07-14 22:13:37
tags:
  - Mac
  - Java
categories: Mac
cover: https://alist.aixcc.top/d/OneDrive/img/202407151124663.webp
---

# Mac 上的 JDK 的安装与卸载

## 从 AdoptOpenJDK 到 Temurin

## 卸载 AdoptOpenJDK
如果您的系统中安装了 AdoptOpenJDK，并且想要替换或升级 JDK 版本，可以按照以下步骤进行卸载：

### 步骤 1：卸载 AdoptOpenJDK
打开终端，并使用 Homebrew Cask 进行卸载。如果您尚未安装 Homebrew，请访问 [Homebrew 安装指南](https://blog.aixcc.top/2024/07/14/安装Homebrew/) 获取详细的安装教程。以卸载 adoptopenjdk8 为例，输入以下命令：
```bash
brew remove --cask adoptopenjdk8
```
重复上述命令，替换 `adoptopenjdk8` 为其他版本号以卸载其他版本的 JDK。

### 步骤 2：移除 Homebrew 的 Tap
完成所有版本的卸载后，执行以下命令来移除 AdoptOpenJDK 的 tap：
```bash
brew untap AdoptOpenJDK/openjdk
```
这样就和 AdoptOpenJDK 完成了告别。

## 清除旧的 Oracle JDK
对于仍在使用 Oracle JDK 的用户，也是时候更新了。请按照以下步骤从您的系统中彻底清除 Oracle JDK：

### 步骤 3：删除旧的 JDK 文件
删除 `/Library/Java/JavaVirtualMachines/` 目录下的 JDK 文件夹。此外，清理以下位置的内容：
- `/Library/Internet Plug-Ins/JavaAppletPlugin.plugin`
- `/Library/PreferencePanes/JavaControlPanel.prefPane`
- `~/Library/Application Support/Oracle/Java`

## 安装 Temurin JDK
在清除旧的 JDK 之后，我们将安装 Temurin，这是 AdoptOpenJDK 的继任者，由 Eclipse Foundation 维护。

### 步骤 4：配置 Homebrew Cask 版本
首先，确保你的 Homebrew 能够访问所有 cask 版本：
```bash
brew search temurin
```
![](https://alist.aixcc.top/d/OneDrive/img/202407151217190.webp)
### 步骤 5：安装 Temurin
现在，您可以安装所需版本的 Temurin。例如，要安装 Temurin@8，运行以下命令：
```bash
brew install --cask temurin@8
```


## 结论
通过以上步骤，您可以在 Mac 上轻松切换 JDK 版本。无论是卸载旧的 AdoptOpenJDK 还是安装新的 Temurin JDK，都能确保您的开发环境与 Java 的最新进展保持同步。

