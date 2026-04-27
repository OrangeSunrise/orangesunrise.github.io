---
title: 开始教程
description: 本文带你快速完成 Chirpy 站点初始化、配置与基础使用。
author: cotes
date: 2019-08-09 20:55:00 +0800
categories: [博客, Chirpy]
tags: [getting started]
---

> 在本全面的概述中开始学习 Chirpy 的基础知识。您将学习如何安装、配置和使用您的第一个基于 Chirpy 的网站，以及如何将其部署到网络服务器上。

> 官方文章翻译版，版本：chirpy-7.4.1
{: .prompt-info }

## 创建站点仓库

在创建你的网站仓库时，根据你的需求有两种选择：

### 方案一：使用 Starter（推荐）

该方式可以简化升级流程、隔离不必要的文件，非常适合希望**专注写作、尽量少做配置**的用户。

1. 登录 GitHub，访问 [**starter**][starter] 仓库。
2. 点击 Use this template 按钮，然后选择 Create a new repository。
3. 将新仓库命名为 `<username>.github.io`，其中 `username` 替换为你的 **GitHub 用户名（小写）**。

------

### 方案二：Fork 主题仓库

这种方式适合需要**深度修改功能或 UI 设计**的用户，但在主题升级时会遇到较多问题。
**如果你不熟悉 Jekyll 或不打算大量定制主题，不建议使用该方式。**

1. 登录 GitHub。
2. Fork 主题仓库：https://github.com/cotes2020/jekyll-theme-chirpy/fork
3. 将仓库命名为 `<username>.github.io`。

------

## 环境搭建

仓库创建完成后，就可以开始配置本地开发环境了。主要有两种方式：

### 使用 Dev Containers（Windows 用户推荐）

Dev Containers 基于 Docker 提供隔离的开发环境，避免与你本机系统发生依赖冲突，所有依赖均在容器中管理。

**步骤：**

1. 安装 Docker：
   - Windows / macOS：安装 [Docker Desktop][docker-desktop]
   - Linux：安装 [Docker Engine][docker-engine]
2. 安装 [VS Code][vscode] 以及 [Dev Containers 扩展][dev-containers]。
3. 克隆你的仓库：
   - 使用 Docker Desktop：在 VS Code 中 [直接克隆仓库到容器卷][dc-clone-in-vol]
   - 使用 Docker Engine：先将仓库克隆到本地，然后通过 VS Code [在容器中打开][dc-open-in-container]
4. 等待 Dev Containers 环境初始化完成。

------

### 本地原生环境搭建（类 Unix 系统推荐）

对于 Linux / macOS 等类 Unix 系统，原生方式通常具有更好的性能。当然，你也可以选择使用 Dev Containers。

**步骤：**

1. 按照 [Jekyll 官方安装文档](https://jekyllrb.com/docs/installation/) 安装 Jekyll，并确保已安装 [Git](https://git-scm.com/)。
2. 将你的仓库克隆到本地。
3. 如果你是 **Fork 主题仓库** 的方式：
   - 安装 [Node.js][nodejs]
   - 在项目根目录执行 `bash tools/init.sh` 初始化仓库
4. 在项目根目录执行 `bundle` 安装 Ruby 依赖。

------

## 使用说明

### 启动 Jekyll 本地服务

在项目根目录执行：

```terminal
$ bundle exec jekyll serve
```

> 如果你使用的是 Dev Containers，必须在 **VS Code 的终端中** 执行该命令。
{: .prompt-info }

几秒后，本地站点将运行在：
[http://127.0.0.1:4000](http://127.0.0.1:4000/)

------

### 配置站点

根据需要修改 `_config.yml`{: .filepath} 中的配置项，常见包括：

- `url`
- `avatar`
- `timezone`
- `lang`

------

### 社交联系方式配置

侧边栏底部显示的社交联系方式可以在 `_data/contact.yml`{: .filepath} 中启用或禁用。

------

### 自定义样式表

如需自定义样式，可将主题中的
`assets/css/jekyll-theme-chirpy.scss`{: .filepath}
复制到你站点中的相同路径，并在文件末尾添加自定义 CSS。

------

### 自定义静态资源

静态资源配置在 `5.1.0` 版本中引入，其 CDN 定义在
`_data/origin/cors.yml`{: .filepath}。

你可以根据站点部署地区的网络情况，替换其中的部分资源。

如果你更倾向于**自托管静态资源**，可参考仓库：
https://github.com/cotes2020/chirpy-static-assets

------

## 部署

在部署前，请确认 `_config.yml`{: .filepath} 中的 `url` 配置正确。

如果你使用的是 **项目站点（project site）**，且没有使用自定义域名，或希望通过非 GitHub Pages 的 Web 服务器并使用子路径访问站点，请务必设置 `baseurl`，例如：

```text
/project-name
```

接下来，请在以下方式中 **选择一种** 进行部署。

------

### 使用 GitHub Actions 部署（推荐）

部署前准备事项：

- 如果你使用的是 GitHub Free 账户，仓库需保持为 **Public**

- 如果仓库中已提交 `Gemfile.lock`{: .filepath}，且你的本地环境不是 Linux，需要执行：

  ```console
  $ bundle lock --add-platform x86_64-linux
  ```

然后配置 GitHub Pages：

1. 进入 GitHub 仓库 → **Settings** → **Pages**
   在 **Source（Build and deployment）** 中选择
   **GitHub Actions** 作为构建来源。
2. 向仓库推送任意提交即可触发 **Build and Deploy** 工作流。
   在 **Actions** 页面可以看到构建过程，成功后站点会自动部署。

完成后，即可通过 GitHub 提供的 URL 访问你的网站。

------

### 手动构建与部署

如果你使用的是自托管服务器，需要在本地构建站点，然后将生成文件上传至服务器。

在项目根目录执行：

```console
$ JEKYLL_ENV=production bundle exec jekyll b
```

默认情况下，生成的静态文件位于 `_site`{: .filepath} 目录，将该目录内容上传到服务器即可。

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[docker-desktop]: https://www.docker.com/products/docker-desktop/
[docker-engine]: https://docs.docker.com/engine/install/
[vscode]: https://code.visualstudio.com/
[dev-containers]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers
[dc-clone-in-vol]: https://code.visualstudio.com/docs/devcontainers/containers#_quick-start-open-a-git-repository-or-github-pr-in-an-isolated-container-volume
[dc-open-in-container]: https://code.visualstudio.com/docs/devcontainers/containers#_quick-start-open-an-existing-folder-in-a-container