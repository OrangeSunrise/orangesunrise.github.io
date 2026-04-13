---
layout: post
title: npm-pnpm 安装指南
date: 2026-04-13 14:33 +0800
author: cerberus
categories: [软件安装, npm-pnpm]
tags: [npm, pnpm]
description:
---

## 一、Ubuntu 22.04 安装 npm

### 方式一：使用 NVM 安装（推荐）

> 官方链接：[Node.js — 下载 Node.js®](https://nodejs.org/zh-cn/download)

```bash
# 下载并安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# 代替重启 shell
\. "$HOME/.nvm/nvm.sh"

# 下载并安装 Node.js（自带 npm）
nvm install 24

# 验证版本
node -v  # 应输出 "v24.x.x"
npm -v   # 应输出对应版本号
```

### 方式二：使用压缩包手动安装

如果 GitHub 连接失败（如 `Connection reset by peer`），可以下载 `node-v24.x.x-linux-x64.tar.xz` 后手动安装。

#### 1. 解压并安装

```bash
# 解压
tar -xvf node-v24.x.x-linux-x64.tar.xz

# 移动到公共目录
sudo mkdir -p /usr/local/lib/nodejs
sudo mv node-v24.x.x-linux-x64 /usr/local/lib/nodejs/node-v24

# 编辑配置文件
nano ~/.bashrc
```

在文件末尾添加：

```bash
# Node.js
export NODE_HOME=/usr/local/lib/nodejs/node-v24
export PATH=$NODE_HOME/bin:$PATH
```

> `Ctrl + O` 保存，`Enter` 确认，`Ctrl + X` 退出。

#### 2. 使配置生效并验证

```bash
source ~/.bashrc
node -v
npm -v
```

**优点**：不依赖网络，不污染系统全局目录。
**缺点**：换版本需手动修改路径。

### 备选：NVM 国内镜像安装

如果 GitHub 连不上，可使用 Gitee 镜像：

```bash
git clone https://gitee.com/mirrors/nvm.git ~/.nvm
cd ~/.nvm
git checkout v0.40.1
source nvm.sh

# 配置 NVM 使用淘宝镜像下载 Node
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node
nvm install 24
```

### 设置 NVM 持久化

安装 nvm 后，如果关闭终端再打开发现 `node -v` 失效，需要将配置写入 Shell 配置文件。

**Bash 用户：**

```bash
cat << 'EOF' >> ~/.bashrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node
EOF
source ~/.bashrc
```

**Zsh 用户：**

```bash
cat << 'EOF' >> ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
export NVM_NODEJS_ORG_MIRROR=https://npmmirror.com/mirrors/node
EOF
source ~/.zshrc
```

设置默认版本，防止重启后失效：

```bash
nvm alias default 24
```

---

## 二、Windows / 通用 npm 安装

### 下载 Node.js

> 官方链接：[Node.js — 下载 Node.js®](https://nodejs.org/zh-cn/download)

下载安装包后运行即可，Node.js 已自带 npm。

---

## 三、npm 镜像源配置

### 查看当前源

```powershell
npm config get registry
```

### 切换镜像源

```powershell
# 切换为淘宝镜像
npm config set registry https://registry.npmmirror.com

# 切换回官方源
npm config set registry https://registry.npmjs.org/
```

### 常用镜像源

| 源名称 | 地址 |
|--------|------|
| 官方默认 | `https://registry.npmjs.org/` |
| 淘宝镜像 | `https://registry.npmmirror.com/` |
| 腾讯云 | `https://mirrors.cloud.tencent.com/npm/` |
| 华为云 | `https://mirrors.huaweicloud.com/repository/npm/` |

### 查看 npm 配置

```powershell
# 查看所有配置
npm config list
# 或
npm config ls

# 查看指定配置
npm config get [key]

# 示例
npm config get registry
npm config get proxy
npm config get https-proxy
```

> **缓存说明**：npm 默认缓存和全局包位置对大多数用户足够，如果默认设置工作良好，不需要额外配置。

---

## 四、pnpm 安装与配置

### 安装

```powershell
npm install -g pnpm
```

### 换源

```powershell
# 查看当前源
pnpm config get registry

# 切换为淘宝镜像
pnpm config set registry https://registry.npmmirror.com
```
