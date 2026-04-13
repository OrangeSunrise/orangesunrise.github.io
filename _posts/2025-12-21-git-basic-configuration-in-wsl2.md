---
layout: post
title: WSL2 中的 Git 基础配置指南
date: 2025-12-21 11:02 +0800
author: cerberus
categories: [软件安装, git]
tags: [wsl2, git]
---

> 本文将以 Ubuntu（WSL2） 为例，完整介绍 Git 的基础配置流程。

---

## WSL2 中的 Git 基础配置指南

在开始使用 **Chirpy + Jekyll** 搭建和维护博客之前，请确保你已经在 **WSL2 / Linux 环境** 中完成了 Git 的基础配置。

> **这一步只需要做一次**
> 后续包括博客初始化、文章提交、主题更新、GitHub Pages 部署等操作，都会依赖 Git。
{: .prompt-info }

本文将以 **Ubuntu（WSL2）** 为例，完整介绍 Git 的基础配置流程。

---

## 一、确认 Git 是否已安装

首先，在 WSL2 终端中检查 Git 是否已安装：

```bash
git --version
```

如果能看到类似下面的输出，说明 Git 已经安装：

```text
git version 2.34.1
```

如果未安装，请执行：

```bash
sudo apt update
sudo apt install -y git
```

---

## 二、配置 Git 用户名和邮箱（必做）

Git 会在每一次提交（commit）中记录作者信息，因此必须先设置 **用户名** 和 **邮箱**。

### 1. 设置用户名

```bash
git config --global user.name "Your Name"
```

示例：

```bash
git config --global user.name "OrangeSunrise"
```

### 2. 设置邮箱

```bash
git config --global user.email "your_email@example.com"
```

> 建议使用 **GitHub 账号绑定的邮箱**，否则在 GitHub 上可能无法正确显示提交者信息。
{: .prompt-tip }

### 3. 验证配置是否成功

```bash
git config --global --list
```

你应该能看到类似内容：

```text
user.name=OrangeSunrise
user.email=orangesunrise@example.com
```

---

## 三、配置 GitHub 访问方式

Git 与 GitHub 通信有两种常见方式：

* **SSH（推荐）**
* HTTPS（需要频繁输入 Token）

在 WSL2 + Chirpy 的使用场景中，**强烈推荐使用 SSH**。

---

## 四、使用 SSH 方式连接 GitHub（推荐）

### 1. 检查是否已有 SSH Key

```bash
ls ~/.ssh
```

如果看到以下文件之一，说明已经存在 SSH Key：

* `id_rsa`
* `id_ed25519`

如果没有，请继续下一步。

---

### 2. 生成新的 SSH Key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

一路回车即可（默认路径和无密码）。

生成完成后，你会看到类似提示：

```text
Your public key has been saved in ~/.ssh/id_ed25519.pub
```

---

### 3. 启动 ssh-agent 并添加私钥

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

### 4. 将 SSH 公钥添加到 GitHub

查看公钥内容：

```bash
cat ~/.ssh/id_ed25519.pub
```

复制输出的**整行内容**，然后：

1. 打开 GitHub
2. 进入 **Settings → SSH and GPG keys**
3. 点击 **New SSH key**
4. 粘贴公钥内容并保存

---

### 5. 测试 SSH 连接

```bash
ssh -T git@github.com
```

如果看到类似提示，说明配置成功：

```text
Hi OrangeSunrise! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 五、WSL2 中 Git 与 Windows Git 的关系说明

> **重要说明**

WSL2 中的 Git 与 Windows 系统中的 Git 是 **完全独立的**：

* WSL2：`/home/username/` 下的 Git 配置
* Windows：`C:\Users\username\` 下的 Git 配置

因此：

* ✅ **WSL2 中必须单独配置 Git**
* ❌ Windows 中的 Git 配置不会自动生效

---

## 六、推荐的 Git 基础习惯（用于博客）

在使用 Chirpy 写博客时，建议遵循以下 Git 使用习惯：

* 一个功能 / 一篇文章 = 一次提交
* 提交信息使用清晰的英文或规范前缀，例如：

  * `feat: add new post about jekyll`
  * `docs: update chirpy configuration`
  * `chore: update site config`

示例：

```bash
git add .
git commit -m "feat: add wsl2 git setup tutorial"
git push
```

---

## 七、下一步你可以做什么？

完成 Git 基础配置后，你已经具备了：

* ✅ 使用 Git 管理博客源码
* ✅ 向 GitHub 推送代码
* ✅ 触发 GitHub Actions 自动部署 Chirpy 博客

接下来你可以继续：

* 初始化 Chirpy 博客仓库
* 编写第一篇博客文章
* 配置 GitHub Pages 自动部署
