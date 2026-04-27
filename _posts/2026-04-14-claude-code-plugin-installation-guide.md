---
layout: post
title: Claude Code安装文档处理插件
date: 2026-04-14 09:21 +0800
author: cerberus
categories: [Claude, Claude Code]
tags: [claude, claude skills]
description: 本文介绍 Claude Code 文档处理插件的安装条件与操作步骤。
---

> 这个插件的地址在国外，需要挂vpn，并且开启全局代理，保证终端可以`ping www.google.com`成功。

## 猫猫云vpn操作

[猫猫云开启全局代理](https://orangesunrise.github.io/posts/maomaoyun-global-proxy-setup/)

太好了！有 VPN 就简单多了，不需要任何国内镜像或中转配置。

## 🚀 最简单的安装方式

### 第一步：确保 Claude Code 已安装

```bash
claude --version
```

如果没安装，查看[Claude Code 安装配置](https://orangesunrise.github.io/posts/a-complete-guide-to-installing-and-configuring-claude-code/)。

### 第二步：启动并安装插件

```bash
# 启动 Claude Code
claude
```

然后在 Claude Code 的交互界面中，依次执行：

```bash
# 1. 添加官方插件市场
# ssh添加方式
/plugin marketplace add anthropic/skills
# 或者使用http添加方式
/plugin marketplace add https://github.com/anthropics/skills.git

# 2. 安装文档处理全家桶（Word、PPT、Excel、PDF）
/plugin install document-skills@anthropic-skills
```

### 完成！✅

就这么简单，两条命令搞定。

## 📝 完整操作示例

启动后你会看到 `>` 提示符，像这样操作：

```shell
D:\Claude> claude

> /plugin marketplace add anthropic/skills
✓ Marketplace added successfully

> /plugin install document-skills@anthropic-skills  
✓ Plugin installed successfully

> 
```

## 💡 验证安装

```bash
# 查看已安装的插件
/plugin list
```

应该能看到 `document-skills` 在列表中。

## 🎯 开始使用

直接自然语言提问即可：

- "帮我创建一个 Word 文档"
- "把这个数据做成 PPT"
- "提取 PDF 里的文字"

有 VPN 的情况下，所有下载和认证都会自动完成，不需要任何额外配置。
