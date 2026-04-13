---
layout: post
title: Claude Code 安装配置
date: 2026-04-13 16:58 +0800
author: cerberus
categories: [Claude, Claude Code]
tags: [claude, claude code]
description:
---

## vscode安装

略

## nodejs安装

[npm-pnpm 安装指南 | Cerberus](https://orangesunrise.github.io/posts/npm-pnpm-installation-guide/)

## git安装

- win11: https://orangesunrise.github.io/posts/windows-11-git-installation/
- wsl2: https://orangesunrise.github.io/posts/git-basic-configuration-in-wsl2/

## claude code安装

```bash
# 使用npm安装claude

npm install -g @anthropic-ai/claude-code
```

## 方案1 使用Claude订阅官方模型配置

![image-20260413164203998](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260413164203998.png)

选择第一个。

![image-20260413164237056](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260413164237056.png)

## 方案2 使用qwen模型配置

> 阿里云官方教程：https://help.aliyun.com/zh/model-studio/claude-code#2bc57cc37cxs2

> 这里以qwen 3.6 plus模型为例。

### 获取qwen模型api

> 阿里云官方教程：[新人免费额度与计费FAQ-大模型服务平台百炼-阿里云-大模型服务平台百炼(Model Studio)-阿里云帮助中心](https://help.aliyun.com/zh/model-studio/new-free-quota)
>
> 模型控制台：[大模型服务平台百炼控制台](https://bailian.console.aliyun.com/cn-beijing?spm=a2c4g.11186623.0.0.f6687f213fWkHR&tab=model#/model-market)

点击[大模型服务平台百炼控制台](https://bailian.console.aliyun.com/cn-beijing?spm=a2c4g.11186623.0.0.f6687f213fWkHR&tab=model#/model-market)

![image-20260409153901150](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260409153901150.png)

选择`api key`，创建自己的key。

![image-20260409154012719](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260409154012719.png)

<img src="https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260409154016421.png" alt="image-20260409154016421" style="zoom:50%;" />

信息可填可不填，然后点击确定，一定要保存好api key，不要忘记了，最好放在记事本保存起来。

### ubuntu24.04配置方法

1. **打开配置文件**（Ubuntu 默认使用 Bash）：

   ```Bash
   nano ~/.bashrc
   ```

   *注：如果你使用的是 Zsh，请修改 `~/.zshrc`。*

2. **在文件末尾添加以下内容**：

   ```Bash
   # Jianhui Claude Configuration
   export ANTHROPIC_BASE_URL=https://dashscope.aliyuncs.com/apps/anthropic
   export ANTHROPIC_API_KEY="你的实际Key内容"
   export ANTHROPIC_MODEL=qwen3.6-plus
   ```
   
3. **保存并退出**：

   按 `Ctrl + O` 保存，按 `Enter` 确认，再按 `Ctrl + X` 退出。

4. **使配置立即生效**：

   ```Bash
   source ~/.bashrc
   ```

5. **在终端中执行下列命令，查看环境变量是否生效：**

   ```bash
   echo $ANTHROPIC_BASE_URL
   echo $ANTHROPIC_API_KEY
   echo $ANTHROPIC_MODEL
   ```

### windows配置方法

1. 打开 **命令提示符 (CMD)**，依次输入以下三条命令：

   ```bash
   setx ANTHROPIC_BASE_URL "https://dashscope.aliyuncs.com/apps/anthropic"
   setx ANTHROPIC_API_KEY "你的实际Key内容"
   setx ANTHROPIC_MODEL "qwen3.6-plus"
   ```

2. **生效机制**：

   - 使用 `setx` 设置后，**必须关闭当前的终端窗口并重新打开一个新的窗口**，环境变量才会生效。
   - 如果你在IDE（如PyCharm, VS Code）中运行代码，通常也需要重启IDE才能读取到新的系统环境变量。

3. 打开一个新的 CMD 窗口，运行以下命令，检查环境变量是否生效：

   ```cmd
   echo %ANTHROPIC_API_KEY%
   echo %ANTHROPIC_BASE_URL%
   echo %ANTHROPIC_MODEL%
   ```

### 运行 claude code

进入项目目录，在终端执行`claude`命令。

```cmd
claude
```

初次使用 claude code 时，可能会强制要求登录 Anthropic 账户。请按以下步骤操作以跳过该流程：

<img src="https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199p1049602.png" alt="img" style="zoom:80%;" />



1. 定位用户主目录下的 `.claude.json` 文件，具体路径如下：

   - macOS / Linux: `~/.claude.json`
   - Windows: `C:\Users\%USERNAME%\.claude.json`

2. 将`hasCompletedOnboarding` 字段的值设置为 `true`，如果没有这个字段添加即可

   ```json
   {
     "hasCompletedOnboarding": true
   }
   ```

3. 保存文件，然后在新终端中重新运行 `claude`。

4. 如果vscode的claude插件还是要求登录，将这个插件卸载重新安装，重新打开vscode即可。

   ```bash
   /home/jianhuichang/.vscode-server/extensions
   
   jianhuichang@ubuntubt:~/.vscode-server/extensions$ ll
   total 112
   drwx------ 24 jianhuichang jianhuichang  4096  4月 10 10:14 ./
   drwxrwxr-x  6 jianhuichang docker        4096  4月 10 10:14 ../
   drwxrwxr-x  4 jianhuichang jianhuichang  4096  4月 10 08:47 anthropic.claude-code-2.1.98-linux-x64/
   ```

   删除`anthropic.claude-code-2.1.98-linux-x64/`这个文件夹。


### 更多配置模型的方式（可选）

Claude Code 支持以下模型配置方式，**按优先级从高到低排列**，优先级高的配置会覆盖优先级低的配置。

1. **对话期间**：执行`/model <模型名称>`命令切换模型。适用于临时切换模型。

   ```plaintext
   /model qwen3.6-plus
   ```

2. **启动 claude code 时**：执行`claude --model <模型名称>`指定模型。适用于单次会话。

   ```plaintext
   claude --model qwen3.6-plus
   ```

3. **设置环境变量**：可按任务复杂度配置不同级别的模型，Claude Code 会根据任务类型自动选择合适的模型。适用于全局生效。

   ```plaintext
   export ANTHROPIC_DEFAULT_OPUS_MODEL="qwen3.6-plus"
   export ANTHROPIC_DEFAULT_SONNET_MODEL="qwen3.6-plus"
   export ANTHROPIC_DEFAULT_HAIKU_MODEL="qwen3-coder-next"
   ```

   其中：

   - `ANTHROPIC_DEFAULT_OPUS_MODEL`：用于复杂推理、架构设计等高难度任务。
   - `ANTHROPIC_DEFAULT_SONNET_MODEL`：用于代码编写、功能实现等日常任务。
   - `ANTHROPIC_DEFAULT_HAIKU_MODEL`：用于语法检查、文件搜索等简单任务。

4. **在 setting.json 配置文件中永久设置**：在项目根目录或用户主目录创建`settings.json`文件，并写入模型配置信息，可分别进行项目级或用户级的永久配置。

   ```json
   {
     "env": {
       "ANTHROPIC_DEFAULT_OPUS_MODEL": "qwen3.6-plus",
       "ANTHROPIC_DEFAULT_SONNET_MODEL": "qwen3.6-plus",
       "ANTHROPIC_DEFAULT_HAIKU_MODEL": "qwen3-coder-next"
     }
   }
   ```

### 错误码

![image-20260409163022432](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260409163022432.png)

### 常见问题

![image-20260409163051394](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260409163051394.png)

**问题3：vscode中的claude插件需要登录**

打开vscode中的claude插件设置选项：

![image-20260413164909441](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260413164909441.png)

![image-20260413165457087](https://cloud-080910t-1316343199.cos.ap-beijing.myqcloud.com/cloud-080910t-1316343199image-20260413165457087.png)

在这里添加`ANTHROPIC_BASE_URL, ANTHROPIC_API_KEY, ANTHROPIC_MODEL`三个配置的内容。
