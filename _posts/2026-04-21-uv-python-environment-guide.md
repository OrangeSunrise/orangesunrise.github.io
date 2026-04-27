---
layout: post
title: uv 与 Python 环境配置指南
date: 2026-04-21 09:34 +0800
author: cerberus
categories: [Python, uv]
tags: [Python, uv]
description: 本文系统讲解使用 uv 管理 Python 版本、环境与项目依赖的方法。
---

> **适用场景**：在 Ubuntu 或 Windows 上使用 `uv` 安装与管理 Python、管理项目依赖、配置镜像源与常见问题排查。

---

## 目录

1. [uv 简介](#1-uv-简介)
2. [安装 uv](#2-安装-uv)
3. [配置镜像源（换源）](#3-配置镜像源换源)
4. [安装与管理 Python](#4-安装与管理-python)
5. [用 uv 管理项目依赖](#5-用-uv-管理项目依赖)
6. [生产依赖与开发依赖的区别](#6-生产依赖与开发依赖的区别)
7. [下载速度与镜像验证](#7-下载速度与镜像验证)
8. [常见问题排查](#8-常见问题排查)
9. [最小实践模板](#9-最小实践模板)
10. [官方文档参考汇总](#10-官方文档参考汇总)
11. [总结](#11-总结)

---

## 1. uv 简介

`uv` 是由 Astral 开发的现代 Python 包管理器与项目管理工具，核心理念是“**项目即环境**”。相比传统的 `pip + venv` 组合，`uv` 提供了更快的安装速度、更可靠的环境隔离和更清晰的项目管理方式。

| **动作** | **传统方式 (pip/venv)**         | **现代方式 (uv Project)**                     |
| -------- | ------------------------------- | --------------------------------------------- |
| 创建环境 | `python -m venv .venv`          | 无需手动创建，`uv run` 或 `uv add` 时自动生成 |
| 记录包   | `pip freeze > requirements.txt` | **自动记录**在 `pyproject.toml` 和 `uv.lock`  |
| 运行代码 | 必须先 `activate` 虚拟环境      | 直接 `uv run`，无需手动激活                   |
| 切换版本 | 重新创建虚拟环境                | `uv python pin 3.x` 然后 `uv sync`            |

---

## 2. 安装 uv

> **官方文档参考**：[安装指南](https://docs.astral.sh/uv/getting-started/installation/)

### 2.1 Ubuntu 24.04

#### 在线安装（推荐）

使用官方安装脚本：

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
# 或（无 curl 时）
wget -qO- https://astral.sh/uv/install.sh | sh
```

安装脚本会自动将 `uv` 放置到 `~/.local/bin`。刷新 shell 使命令生效：

```bash
source ~/.bashrc
```

#### 离线安装

1. 在有网络的机器上下载离线包：

   ```bash
   # 从 GitHub Releases 下载对应平台的 tar.gz 包
   # 例如：uv-x86_64-unknown-linux-gnu.tar.gz
   # 下载链接：https://github.com/astral-sh/uv/releases
   ```

2. 将压缩包传输到目标机器后解压并安装：

   ```bash
   tar -xzf uv-x86_64-unknown-linux-gnu.tar.gz
   sudo mv uv-x86_64-unknown-linux-gnu/uv /usr/local/bin/uv
   sudo chmod +x /usr/local/bin/uv
   ```

验证安装：

```bash
uv --version
```

### 2.2 Windows

#### 在线安装

使用 PowerShell 执行官方安装脚本：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

安装脚本会自动将 `uv.exe` 放置到 `%USERPROFILE%\.local\bin`，并将该目录加入 **Path** 环境变量。重新打开 PowerShell 验证：

```powershell
uv --version
```

#### Windows 离线安装

1. 从 [GitHub Releases](https://github.com/astral-sh/uv/releases) 下载 `uv-x86_64-pc-windows-msvc.zip`。
2. 解压后将 `uv.exe` 放置到自定义目录，例如：`D:\uv_data\bin\uv.exe`。
3. 将 `D:\uv_data\bin` 加入系统 **Path** 环境变量：
   - 右键"此电脑" → 属性 → 高级系统设置 → 环境变量 → 系统变量 Path → 编辑 → 新建 → 填入 `D:\uv_data\bin`。
4. 重新打开 PowerShell 或 CMD，验证：

```powershell
uv --version
```

### 2.3 Windows 自定义路径配置（可选）

如果希望统一管理 `uv` 相关的缓存、Python 解释器和工具路径，可设置以下系统环境变量：

| **变量名**              | **变量值**                                | **作用**                                         |
| ----------------------- | ----------------------------------------- | ------------------------------------------------ |
| `UV_CACHE_DIR`          | `D:\uv_data\cache`                        | 包缓存目录，避免占用系统盘                       |
| `UV_DEFAULT_INDEX`      | `https://mirrors.aliyun.com/pypi/simple/` | 国内镜像源，加速下载                             |
| `UV_PYTHON_INSTALL_DIR` | `D:\uv_data\python`                       | `uv python install` 下载的 Python 解释器存放位置 |
| `UV_TOOL_DIR`           | `D:\uv_data\tools`                        | `uv tool install` 安装的工具存放位置             |

> **注意**：若使用 VPN，建议选择阿里云镜像源，因为部分大学镜像源可能会屏蔽海外节点。

配置完成后，重新打开终端验证：

```powershell
uv --version
Get-ChildItem Env:UV_*
```

---

## 3. 配置镜像源（换源）

> **官方文档参考**：[镜像源配置（Index）](https://docs.astral.sh/uv/concepts/indexes/) · [环境变量参考](https://docs.astral.sh/uv/reference/environment/)

`uv` 支持多个层级配置镜像源，优先级为：**命令行参数 > 项目 `pyproject.toml` > 环境变量**。

### 3.1 临时生效（当前终端）

```bash
export UV_DEFAULT_INDEX="https://mirrors.aliyun.com/pypi/simple/"
```

关闭终端后失效，适合临时测试。

### 3.2 持久生效（用户级）

将上面的 `export` 命令写入 `~/.bashrc` 或 `~/.zshrc`，每次打开终端自动生效。

### 3.3 项目级（推荐团队协作）

在项目根目录 `pyproject.toml` 中写入：

```toml
[[tool.uv.index]]
name = "aliyun"
url = "https://mirrors.aliyun.com/pypi/simple/"
default = true
```

优点：配置随项目纳入版本控制，团队保持一致性，可复现性更强。

### 3.4 环境变量命名更新

旧版本中常见的写法是 `UV_INDEX_URL`，`uv` 现已推荐使用 `UV_DEFAULT_INDEX`。

**建议**：

1. 若项目已在 `pyproject.toml` 中设置 `default = true`，可删除全局的旧环境变量。
2. 若希望所有临时项目默认走镜像源，可设置全局 `UV_DEFAULT_INDEX`。

> `uv` 的设计意图是：默认源应写在项目的 `pyproject.toml` 中，而非依赖每个人电脑上各不相同的环境变量。

---

## 4. 安装与管理 Python

> **官方文档参考**：[Python 版本管理](https://docs.astral.sh/uv/guides/install-python/)

### 4.1 安装 Python

```bash
uv python install            # 安装最新可用版本
uv python install 3.12       # 安装指定版本
uv python install 3.11 3.12  # 一次安装多个版本
```

### 4.2 查看已安装版本与目录

```bash
uv python list   # 列出已安装的 Python 版本
uv python dir    # 显示 Python 安装目录
```

![查看Python安装路径](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199/image-20260326102542083.png)

### 4.3 固定项目 Python 版本

```bash
uv python pin 3.12
```

会在项目中生成或更新 `.python-version` 文件，这是 `uv` 识别项目 Python 版本的最高优先级依据。

也可以在初始化项目时直接指定：

```bash
uv init --python 3.10
```

### 4.4 查看当前项目使用的解释器

```bash
uv python which
```

### 4.5 运行 Python（推荐方式）

```bash
uv run python
```

使用 `uv run` 可避免系统 Python、商店别名、PATH 冲突等问题，是 `uv` 推荐的标准用法。

---

## 5. 用 uv 管理项目依赖

> **官方文档参考**：[项目管理（Projects）](https://docs.astral.sh/uv/getting-started/projects/) · [依赖管理（Dependencies）](https://docs.astral.sh/uv/concepts/projects/dependencies/) · [命令行参考](https://docs.astral.sh/uv/reference/cli/)

在 `uv` 项目模式下，应使用 `uv add` 而非 `uv pip install`。依赖会被自动写入 `pyproject.toml` 并更新 `uv.lock`。

### 5.1 初始化项目

```bash
uv init              # 在当前目录初始化
uv init my-project   # 创建指定名称的项目
cd my-project
```

`uv` 会自动创建 `pyproject.toml` 和 `.python-version` 文件。

### 5.1.1 控制项目 Python 版本

项目初始化后，可以使用 `uv python pin` 锁定项目所需的 Python 版本，确保团队和部署环境的一致性：

```bash
uv python pin 3.12           # 锁定为 3.12.x 的最新补丁版本
uv python pin 3.12.3         # 锁定为精确的 3.12.3 版本
uv python pin 3.10           # 锁定为 3.10.x
```

`pin` 会在项目根目录生成或更新 `.python-version` 文件，该文件应纳入版本控制，以便团队成员拉取代码后获得一致的 Python 版本。

查看当前项目固定的版本：

```bash
cat .python-version
```

固定版本后，执行 `uv sync` 或 `uv run` 时会自动使用指定的 Python 版本创建虚拟环境。如需查看当前项目实际使用的解释器路径：

```bash
uv python which
```

### 5.2 添加/删除依赖

```bash
uv add requests pandas     # 添加生产依赖
uv add --dev pytest ruff   # 添加开发依赖
uv remove requests         # 移除依赖
```

### 5.3 同步环境

从 Git 拉取他人项目后，按 `uv.lock` 精确还原环境：

```bash
uv sync
```

### 5.4 运行脚本与工具

```bash
uv run main.py        # 运行脚本
uv run python         # 交互式终端 (REPL)
uv run pytest         # 运行已安装的工具
```

`uv run` 会自动确保在正确的虚拟环境中执行，无需手动激活 `.venv`。

---

## 6. 生产依赖与开发依赖的区别

加不加 `--dev` 的本质区别：**"生产运行必需" vs "本地开发辅助"**。

### 6.1 概念区别

- **不加 `--dev`（生产依赖 / Production Dependencies）**
  代码在生产环境运行时必须存在的包。例如：`torch`、`torchvision` —— AI 模型代码没有它们无法运行。

- **加上 `--dev`（开发依赖 / Development Dependencies）**
  仅用于本地开发、测试、调试或格式化的工具，部署时不需要。例如：`pytest`（测试）、`ipykernel`（Jupyter 内核）、`matplotlib`（画图）、`ruff`（代码格式化）。

### 6.2 在 `pyproject.toml` 中的体现

- 不加 `--dev` → 写入 `[project]` 下的 `dependencies` 列表
- 加上 `--dev` → 写入 `[tool.uv]` 下的 `dev-dependencies` 列表

### 6.3 工程价值

1. **生产环境更轻量**：部署时仅安装生产依赖，Docker 镜像更小，磁盘占用更少。
2. **安全性更高**：减少不必要的依赖面，降低被扫描出安全漏洞的风险。
3. **团队协作更高效**：新成员可按需安装依赖，拉取代码后按需加载开发工具。

### 6.4 快速判断

- 代码 `import` 了某个包，**服务器上跑也必须 `import`** → **不加 `--dev`**
- 安装包只是为了**本地写笔记、画图、格式化、测试** → **加上 `--dev`**

---

## 7. 下载速度与镜像验证

### 7.1 快速验证

```bash
uv add rich --verbose
```

在 `--verbose` 模式下，观察输出是否使用镜像源地址（如 `Looking in index: https://mirrors.aliyun.com...`）。

### 7.2 运行库自检

```bash
uv run python -m rich
```

若能正常展示彩色终端演示界面（表格、进度条、颜色示例），说明：

1. 网速正常：包已通过镜像源成功下载
2. 环境正常：Python 能够正确调用第三方库

### 7.3 更大量测速（可选）

```bash
uv add pandas
```

`pandas` 依赖较多（numpy、pytz 等），`uv` 会并行下载所有依赖，是观察多线程下载优势的好时机。

### 7.4 测试后清理（可选）

```bash
uv remove rich pandas
```

如果仅用于测试，也可以直接删除 `.venv` 文件夹和 `uv.lock` 文件。

---

## 8. 常见问题排查

### 8.1 Windows 输入 `python` 跳转 Microsoft Store

![Microsoft Store 冲突](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199/image-20260326101816337.png)

**原因**：Windows 自带一个指向 Microsoft Store 的 `python.exe` 占位符（应用执行别名），抢占了命令。

**处理顺序**：

1. **推荐**：使用 `uv run python --version`（最稳妥，符合 `uv` 设计理念）。
2. **关闭别名**：打开 Windows 设置 → 搜索"应用执行别名" → 将 `python.exe` 和 `python3.exe` 设为 **关 (Off)**。
3. **自动关联**（可选）：运行 `uv python update-shell`，然后重启 PowerShell。
4. 重新打开终端验证。

### 8.2 只有 `python3.12`，没有 `python` 命令

![python 命令找不到 - 1](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199/image-20260326104613859.png)
![python 命令找不到 - 2](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199/image-20260326104628157.png)

`uv python update-shell` 有时只生成特定版本的别名（如 `python3` 或 `python3.12`）。

**Windows 解决方法**：

```powershell
# 1. 进入 uv 的 shim 目录
cd C:\Users\JianhuiChang\.local\bin

# 2. 创建硬链接，让 python 指向 python3.12
New-Item -ItemType HardLink -Path ".\python.exe" -Value ".\python3.12.exe"
```

创建后验证：

```powershell
where.exe python
# 预期输出：C:\Users\JianhuiChang\.local\bin\python.exe
```

**删除硬链接**（可随时执行，不影响 `python3.12.exe`）：

```powershell
Remove-Item "C:\Users\JianhuiChang\.local\bin\python.exe"
```

硬链接原理：多个文件名指向同一块存储数据，只有全部删除后原始数据才会被清除。

### 8.3 Ubuntu 需要 `python` 命令

Ubuntu 24.04 默认不提供 `python` 命令（仅有 `python3`）。

**推荐**：优先使用 `python3` 或 `uv run python`。

如确实需要 `python` 命令，可通过以下方式配置：

```bash
# 方法一：在 /usr/local/bin 中创建软链接（推荐）
sudo ln -sf /usr/bin/python3.12 /usr/local/bin/python

# 方法二：使用 update-alternatives 管理（最规范）
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.12 1
sudo update-alternatives --config python
```

> **注意**：不要删除系统自带的 `/usr/bin/python3.12`，只创建软链接即可。Ubuntu 的 `python` 命令缺失是设计决策，手动创建软链接是安全的。

### 8.4 换源后清空缓存

更换镜像源后，建议先清空缓存再安装：

```bash
uv cache clean
```

然后重新打开终端。

---

## 9. 最小实践模板

### 9.1 新项目从零开始

```bash
# 1. 创建并进入项目目录
uv init my-project
cd my-project

# 2. 锁定项目使用的 Python 版本（生成 .python-version 文件）
uv python pin 3.12

# 3. 添加生产依赖（代码运行时必需的包）
uv add requests

# 4. 添加开发依赖（仅本地开发使用的工具）
uv add --dev pytest ruff

# 5. 在 uv 管理的虚拟环境中运行一行测试命令，验证环境正常
uv run python -c "import requests; print('ok')"
```

### 9.2 从 Git 拉取已有项目

```bash
# 1. 克隆远程仓库到本地
git clone <repository-url>

# 2. 进入项目目录
cd <project-dir>

# 3. 根据 pyproject.toml 和 uv.lock 精确还原项目依赖与 Python 版本
uv sync

# 4. 验证当前环境的 Python 版本是否符合项目要求
uv run python --version
```

---

## 10. 官方文档参考汇总

本文各章节已嵌入对应的官方文档链接，方便随时查阅。以下为完整索引：

| **主题**    | **对应章节** | **官方文档链接**                                                                                                                                                                                        |
| ----------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 安装        | 第 2 节      | [Installation](https://docs.astral.sh/uv/getting-started/installation/)                                                                                                                                 |
| 镜像源      | 第 3 节      | [Indexes](https://docs.astral.sh/uv/concepts/indexes/) · [Environment Variables](https://docs.astral.sh/uv/reference/environment/)                                                                      |
| Python 管理 | 第 4 节      | [Install Python](https://docs.astral.sh/uv/guides/install-python/)                                                                                                                                      |
| 项目与依赖  | 第 5 节      | [Projects](https://docs.astral.sh/uv/getting-started/projects/) · [Dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/) · [CLI Reference](https://docs.astral.sh/uv/reference/cli/) |
| GitHub 仓库 | —            | [astral-sh/uv](https://github.com/astral-sh/uv)                                                                                                                                                         |

> `uv` 更新频繁，如遇行为变化，建议以[官方文档首页](https://docs.astral.sh/uv/)为准。

---

## 11. 总结

1. **安装统一**：`uv` 支持在线脚本安装与离线包手动安装，Ubuntu 和 Windows 均有对应方案。
2. **换源推荐**：优先将镜像源配置到项目 `pyproject.toml` 中（`default = true`），提升可复现性。
3. **Python 管理**：使用 `uv python install/pin/list/which` 管理解释器，运行统一使用 `uv run python`。
4. **依赖管理**：统一使用 `uv add / uv remove / uv sync`，将 `--dev` 与生产依赖分离，通过 `uv python pin` 锁定项目 Python 版本。
5. **排查思路**：遇到命令冲突时，优先使用 `uv run` 绕过，再视情况配置系统别名。
6. **官方文档**：`uv` 更新频繁，具体行为以[官方文档](https://docs.astral.sh/uv/)为准。
