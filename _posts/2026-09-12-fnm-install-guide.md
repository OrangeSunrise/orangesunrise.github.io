---
layout: post
title: fnm 安装指南
date: 2026-09-12 16:02 +0800
author: cerberus
categories: [软件安装, fnm]
tags: [fnm, nodejs]
description: 本文介绍 fnm 的安装、换源及常见问题处理方法。
---

## Ubuntu 24.04 安装 fnm

- 官网连接：[fnm](https://github.com/Schniz/fnm)
- 相关文档：[fnm 文档](https://www.fnmnode.com/zh-cn/)

### 使用安装脚本安装（推荐）

> 确保`curl`和`unzip`已安装
{: .prompt-tip }

```bash
curl -fsSL https://fnm.vercel.app/install | bash
```

安装后刷新终端：

```bash
source ~/.bashrc
```

### 更换fnm源

```bash
# 查看当前源
fnm env
# 更换为国内源
nano ~/.bashrc
# 在文件末尾添加：
export FNM_NODE_DIST_MIRROR=https://npmmirror.com/mirrors/node/
# 保存并刷新终端
source ~/.bashrc
```

### fnm安装nodejs

```bash
# 安装指定版本的nodejs
fnm install 24
# 查看已安装的nodejs版本
fnm list
# 切换nodejs版本
fnm use 24
# 查看当前使用的nodejs版本
node -v 
```

### 更换npm源

```bash
# 查看当前npm源
npm config get registry
# 更换为国内源
npm config set registry https://registry.npmmirror.com/
# 查看更换后的npm源
npm config get registry
```

## windows 安装 fnm

使用winget安装fnm：

```shell
winget install Schniz.fnm
```

> 安装后重新启动终端
{: .prompt-warning }

### 更换fnm源

```shell
# 查看当前源
fnm env
# 更换为国内源
setx FNM_NODE_DIST_MIRROR https://npmmirror.com/mirrors/node
# 重新启动终端
```

### fnm安装nodejs

```shell
# 安装指定版本的nodejs
fnm install 24
# 查看已安装的nodejs版本
fnm list
# 切换nodejs版本
fnm use 24
# 查看当前使用的nodejs版本  
node -v 
```

### 更换npm源

```shell
# 查看当前npm源
npm config get registry
# 更换为国内源
npm config set registry https://registry.npmmirror.com/
# 查看更换后的npm源
npm config get registry
```
