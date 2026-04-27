---
layout: post
title: Win11安装git
date: 2026-04-13 15:53 +0800
author: cerberus
categories: [软件安装, git]
tags: [win11, git]
description: 本文说明在 Windows 11 安装 Git 并集成 PowerShell 的关键设置。
---

> 在 Windows 11 上将 Git 终端集成到 PowerShell 中，关键是安装时正确设置 PATH 环境变量，安装后配合 `posh-git` 增强提示和补全功能。以下是完整的操作指南：

---

## 一、下载与安装 Git

### 1.1 下载 Git 安装程序
访问[Git官方网站](https://git-scm.com/downloads)或[清华源git下载链接](https://mirrors.tuna.tsinghua.edu.cn/github-release/git-for-windows/git/LatestRelease/Git-2.53.0.2-64-bit.exe)，页面会自动识别 Windows 系统并提供下载。下载的文件名类似 `Git-2.52.0-64-bit.exe` 。

### 1.2 运行安装程序（关键步骤）
找到下载的 `.exe` 文件，**右键选择“以管理员身份运行”**，以避免后续权限问题 。

### 1.3 安装过程中的关键选项

| 安装步骤                                    | 推荐选项                                                                     | 说明                                                                            |
| ------------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Select Components**                       | 保持默认，建议勾选 `Add a Git Bash Profile to Windows Terminal`              | 方便在 Win11 自带终端中使用                                                     |
| **Choosing the default editor**             | 选择 `Use Visual Studio Code` 或 `Use Notepad`                               | 不建议选 Vim（新手操作困难）                                                    |
| **Adjusting your PATH environment**         | ✅ **选第二项**：`Git from the command line and also from 3rd-party software` | **这是集成到 PowerShell 的关键**——选此项才能在 PowerShell 中直接调用 `git` 命令 |
| **Choosing HTTPS transport backend**        | 默认 `Use the OpenSSL library`                                               | 保持默认                                                                        |
| **Configuring the line ending conversions** | 默认 `Checkout Windows-style, commit Unix-style line endings`                | 跨平台开发时推荐                                                                |
| **Configuring the terminal emulator**       | 默认 `Use MinTTY`                                                            | 保持默认                                                                        |

### 1.4 完成安装
点击 `Install` 等待安装完成，建议勾选 `Launch Git Bash` 然后点击 `Finish` 。

---

## 二、验证安装与基础配置

### 2.1 验证安装
打开 **PowerShell**（按 `Win` 键，搜索 “PowerShell”），输入以下命令：

```powershell
git --version
```

如果显示类似 `git version 2.52.0.windows.1`，说明安装成功，PATH 环境变量配置正确 。

### 2.2 配置用户名和邮箱（必须）
```powershell
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱@example.com"
```

这两条配置会关联到你的每一次提交记录中 。

### 2.3 验证配置
```powershell
git config --global --list
```

应该能看到刚才设置的用户名和邮箱 。

---

## 三、增强 PowerShell 的 Git 体验（posh-git）

为了让 PowerShell 显示 Git 仓库状态（分支、修改状态等），可以安装 `posh-git` 模块。

### 3.1 设置执行策略（仅需一次）
以**管理员身份**打开 PowerShell，执行：

```powershell
Set-ExecutionPolicy -Scope LocalMachine -ExecutionPolicy RemoteSigned -Force
```

这允许运行本地脚本 。

### 3.2 安装 posh-git 模块
在**普通权限**的 PowerShell 中执行：

```powershell
Install-Module posh-git -Scope CurrentUser -Force
```

安装过程如提示确认，输入 `Y` 即可 。

### 3.3 自动加载 posh-git
```powershell
Add-PoshGitToProfile -AllHosts
```

这条命令会将导入语句添加到你的 PowerShell 配置文件中，以后每次打开 PowerShell 都会自动加载 Git 提示增强功能 。

### 3.4 重启 PowerShell
关闭并重新打开 PowerShell，进入任意 Git 仓库目录，你的命令行提示符就会显示当前分支和文件状态信息。

---

## 四、在 PowerShell 中使用 Git 的示意图

```
PS C:\Users\MyProject [main ≡ +1 ~0 -0]>
     ↑                 ↑
   普通路径          Git状态提示
                    (分支:main, 1个未暂存文件)
```

这种状态提示可以让你在不输入 `git status` 的情况下，快速了解仓库状态。

---

## 五、常见问题排查

| 问题                                     | 解决方法                                                                                                |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `git --version` 提示“不是内部或外部命令” | 说明 PATH 配置有误。重新运行 Git 安装程序，在“Adjusting your PATH environment”步骤**必须选第二项**      |
| PowerShell 无法执行脚本                  | 以管理员身份运行 `Set-ExecutionPolicy RemoteSigned -Force`                                              |
| 安装 posh-git 时报错                     | 先执行 `Install-Module PowerShellGet -Force -SkipPublisherCheck`，再重新安装 posh-git                   |
| 想要在 PowerShell 中直接运行 Bash 脚本   | 使用 `bash -c "你的命令"`，或设置别名 `Set-Alias -Name bash -Value "C:\Program Files\Git\bin\bash.exe"` |

---

## 六、备选安装方式（命令行）

如果你熟悉命令行操作，也可以使用 winget 快速安装：

```powershell
# 以管理员身份打开 PowerShell，执行：
winget install --id Git.Git -e --source winget
```

这种方式会自动完成安装，无需手动点击下一步 。

---

完成以上步骤后，你就可以在 Windows 11 的 PowerShell 中无缝使用 Git 命令，并享受 `posh-git` 带来的状态提示和命令补全功能了。