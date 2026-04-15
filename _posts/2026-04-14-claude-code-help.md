---
layout: post
title: Claude Code Help说明文档
date: 2026-04-14 22:17 +0800
author: cerberus
categories: [Claude, Claude Code]
tags: [claude, claude code help]
description:
---

> 本文档整理自 Claude Code CLI 的 `/help` 界面，涵盖 **General（通用）**、**Commands（内置命令）** 和 **Custom Commands（自定义命令）** 三个部分。

---

## 一、General（通用操作）

### 键盘快捷键

#### 中断与退出

| 快捷键 | 说明 |
| -------- | ------ |
| `Ctrl+C` | 取消当前输入或停止生成 |
| `Ctrl+D` | 退出 Claude Code 会话 |
| `Ctrl+X Ctrl+K` | 终止所有后台 Agent（3秒内按两次确认） |

#### 文本与输入管理

| 快捷键 | 说明 |
| -------- | ------ |
| `Ctrl+L` | 清空提示输入框（保留对话历史） |
| `Ctrl+G` / `Ctrl+X Ctrl+E` | 在默认文本编辑器中编辑当前提示 |
| `Ctrl+V` / `Cmd+V`(iTerm2) / `Alt+V`(Windows) | 从剪贴板粘贴图片，插入为 `[Image #N]` 引用 |

#### 显示与输出

| 快捷键 | 说明 |
| -------- | ------ |
| `Ctrl+O` | 切换详细输出模式（显示工具调用细节，展开 MCP 读取和搜索调用） |
| `Ctrl+T` | 切换任务列表显示/隐藏 |

#### 导航与历史

| 快捷键 | 说明 |
| -------- | ------ |
| `Ctrl+R` | 反向搜索命令历史 |
| `↑ / ↓` 方向键 | 浏览命令历史 |
| `← / →` 方向键 | 在对话框标签页间切换 |

#### 会话与功能控制

| 快捷键 | 说明 |
| -------- | ------ |
| `Esc` + `Esc` | 回退/摘要（恢复到之前的代码或对话节点，或生成摘要） |
| `Shift+Tab` / `Alt+M` | 循环切换权限模式（default → acceptEdits → plan → auto 等） |
| `Ctrl+B` | 将当前任务移至后台运行（tmux 用户需按两次） |
| `Option+P` / `Alt+P` | 切换模型（不清空当前提示） |
| `Option+T` / `Alt+T` | 切换扩展思考模式 |
| `Option+O` / `Alt+O` | 切换快速模式 |

---

### 特殊前缀字符（输入模式）

| 前缀 | 说明 |
| ------ | ------ |
| `/` | 命令/技能模式：输入内置命令或自定义 Skill |
| `!` | Bash 模式：直接执行 Shell 命令，输出加入会话上下文 |
| `@` | 文件路径提及：触发文件路径自动补全，引用文件或目录 |

---

### 多行输入

| 方式 | 说明 |
| ------ | ------ |
| `\` + `Enter` | 通用换行（所有终端支持） |
| `Option+Enter` | macOS 默认 |
| `Shift+Enter` | 在 iTerm2、WezTerm、Ghostty、Kitty 中开箱即用 |
| `Ctrl+J` | 通用换行控制序列 |
| 直接粘贴 | 粘贴代码块或日志 |

---

### 通用使用技巧

- **后台命令**：长时间运行的命令（构建、测试、开发服务器）可用 `Ctrl+B` 移至后台，继续对话不受影响。
- **Bash 模式 `!` 前缀**：直接运行 Shell 命令，支持实时输出、Tab 历史补全、`Ctrl+B` 后台化，按 `Esc`/`Backspace`/`Ctrl+U`（空提示时）退出。
- **提示建议**：界面会显示灰色示例命令（来自 git 历史），按 `Tab`/`→` 接受，按 `Enter` 接受并提交。
- **旁白提问 `/btw`**：可在 Claude 处理任务时提出快速问题，不记入对话历史，不消耗上下文，成本极低。
- **任务列表**：复杂任务时 Claude 自动创建任务列表，按 `Ctrl+T` 显示/隐藏，上下文压缩后仍保留。
- **PR 状态**：底栏显示可点击的 PR 链接，颜色表示状态（绿=已批准，黄=待审，红=需修改，灰=草稿，紫=已合并）。

---

## 二、Commands（内置命令）

### 会话管理

| 命令 | 说明 |
| ------ | ------ |
| `/clear` | 清除对话历史，释放上下文（别名：`/reset`、`/new`） |
| `/resume [session]` | 恢复之前的会话（别名：`/continue`） |
| `/compact [instructions]` | 压缩对话历史，可附加指令指定保留重点 |
| `/rewind` | 回退到之前的节点或生成摘要 |
| `/rename [name]` | 重命名当前会话 |
| `/branch [name]` | 在当前节点创建分支（别名：`/fork`） |
| `/exit` | 退出 Claude Code（别名：`/quit`） |

### 配置与设置

| 命令 | 说明 |
| ------ | ------ |
| `/config` | 打开设置界面（主题、模型、输出风格等）（别名：`/settings`） |
| `/model [model]` | 选择或切换 AI 模型 |
| `/effort [level]` | 设置努力级别（low / medium / high / max / auto） |
| `/permission-mode` | 切换权限模式 |
| `/theme` | 更改配色主题 |
| `/keybindings` | 打开或创建键盘快捷键配置 |
| `/color [color]` | 为当前会话设置提示栏颜色 |
| `/terminal-setup` | 配置终端快捷键（Shift+Enter 等） |
| `/statusline` | 配置状态栏显示 |
| `/voice` | 切换语音听写（Push-to-Talk） |

### 文件与上下文

| 命令 | 说明 |
| ------ | ------ |
| `/add-dir <path>` | 添加工作目录，让 Claude 可访问该路径文件 |
| `/context` | 以彩色网格可视化上下文用量，并提供优化建议 |
| `/copy [N]` | 将最后一条回复复制到剪贴板（可交互选择代码块） |
| `/memory` | 编辑 CLAUDE.md、启用/禁用自动记忆、查看记忆条目 |

### 工具集成

| 命令 | 说明 |
| ------ | ------ |
| `/mcp` | 管理 MCP 服务器连接和 OAuth 认证 |
| `/ide` | 管理 IDE 集成并显示状态 |
| `/hooks` | 查看工具事件的 Hook 配置 |
| `/chrome` | 配置 Chrome 浏览器集成设置 |

### Agents 与技能

| 命令 | 说明 |
| ------ | ------ |
| `/agents` | 管理 Agent/子 Agent 配置 |
| `/skills` | 列出所有可用的 Skill |
| `/plugin` | 管理 Claude Code 插件 |
| `/reload-plugins` | 无需重启即可重载插件 |
| `/plan [description]` | 直接进入计划模式（可附加描述） |
| `/permissions` | 管理工具的允许/询问/拒绝规则（别名：`/allowed-tools`） |

### 扩展功能

| 命令 | 说明 |
| ------ | ------ |
| `/schedule [description]` | 创建/更新/列出/运行云端定时任务 |
| `/security-review` | 分析待合并分支的安全漏洞 |
| `/diff` | 打开未提交变更的交互式 Diff 查看器 |
| `/btw <question>` | 提出旁白问题，不加入对话历史 |
| `/tasks` | 列出并管理后台任务（别名：`/bashes`） |

### 信息与诊断

| 命令 | 说明 |
| ------ | ------ |
| `/status` | 显示版本、模型、账户、连接状态 |
| `/cost` | 显示 Token 用量统计 |
| `/usage` | 显示套餐用量限制和速率限制状态 |
| `/doctor` | 诊断并验证安装和配置问题 |
| `/help` | 显示帮助和可用命令 |
| `/feedback [report]` | 提交反馈（别名：`/bug`） |
| `/release-notes` | 在交互式版本选择器中查看更新日志 |
| `/powerup` | 通过交互式课程探索功能 |
| `/insights` | 生成会话分析报告 |
| `/stats` | 可视化每日用量、会话、连续记录、模型偏好 |

### 账户与订阅

| 命令 | 说明 |
| ------ | ------ |
| `/login` | 登录 Anthropic 账户 |
| `/logout` | 退出登录 |
| `/upgrade` | 升级到更高套餐（Pro/Max） |
| `/passes` | 分享免费访问周（符合条件的账户） |
| `/privacy-settings` | 查看/更新隐私设置（Pro/Max） |

### GitHub 与远程

| 命令 | 说明 |
| ------ | ------ |
| `/install-github-app` | 配置 Claude GitHub Actions |
| `/install-slack-app` | 安装 Claude Slack 应用 |
| `/remote-control` | 启用从 claude.ai 远程控制（别名：`/rc`） |
| `/remote-env` | 配置默认远程环境 |
| `/desktop` | 在桌面应用中继续会话（macOS/Windows）（别名：`/app`） |
| `/teleport` | 在终端中恢复 Web 会话 |
| `/ultraplan <prompt>` | 在浏览器中起草计划并远程执行 |
| `/mobile` | 显示移动端应用二维码（别名：`/ios`、`/android`） |

---

## 三、Custom Commands（自定义命令 / Skills）

### 存放位置

| 位置 | 路径 | 作用范围 |
| ------ | ------ | ---------- |
| 个人全局 | `~/.claude/skills/<名称>/SKILL.md` | 所有项目 |
| 项目专属 | `.claude/skills/<名称>/SKILL.md` | 当前项目 |
| 旧版兼容 | `.claude/commands/<名称>.md` | 仍然支持 |
| 插件 | `<plugin>/skills/<名称>/SKILL.md` | 插件启用的范围 |

> **优先级**：企业配置 > 个人全局 > 项目专属

---

### SKILL.md 文件结构

```yaml
---
name: skill-name               # 命令名（小写+连字符，最多64字符）
description: 何时使用此技能     # 决定 Claude 是否自动调用
disable-model-invocation: true # 禁止 Claude 自动调用（仅手动触发）
user-invocable: false          # 隐藏在 / 菜单（仅 Claude 内部使用）
allowed-tools: Read Grep       # 限制可用工具
argument-hint: "[参数说明]"    # 自动补全提示
model: claude-opus-4-6         # 覆盖模型
effort: high                   # 覆盖努力级别
context: fork                  # 在独立子 Agent 上下文中运行
agent: Explore                 # 指定子 Agent 类型
paths: src/**/*.ts             # 限制自动激活的文件路径
---

技能正文内容（支持 Markdown 和变量替换）...
```

---

### 变量替换

| 变量 | 含义 |
| ------ | ------ |
| `$ARGUMENTS` | 调用时传入的所有参数 |
| `$ARGUMENTS[N]` / `$N` | 按索引取第 N 个参数（从0开始） |
| `${CLAUDE_SESSION_ID}` | 当前会话 ID |
| `${CLAUDE_SKILL_DIR}` | 技能 SKILL.md 所在目录 |

---

### 动态上下文注入

使用 `` !`command` `` 在 Claude 读取技能前注入 Shell 输出：

```markdown
当前 PR Diff: !`gh pr diff`
PR 评论: !`gh pr view --comments`
```

---

### 内置捆绑 Skills（预装）

| 命令 | 说明 |
| ------ | ------ |
| `/batch <instruction>` | 并行大规模代码修改，自动拆分为5-30个独立任务，每个任务创建独立 worktree 并开 PR |
| `/loop [interval] <prompt>` | 按间隔反复执行提示（如 `/loop 5m check deploy`），用于轮询、监控任务 |
| `/simplify [focus]` | 审查最近修改的文件，检查代码质量和可复用性，并自动修复问题（3个并行审查 Agent） |
| `/debug [description]` | 启用调试日志并排查问题 |
| `/claude-api` | 加载 Claude API 参考文档（Python/TS/Go/Java/Ruby 等），当代码导入 `anthropic` 时自动触发 |

---

### 使用示例

**纯手动触发的部署命令：**

```yaml
---
name: deploy
description: 部署到生产环境
disable-model-invocation: true
---

部署 $ARGUMENTS 到生产环境：
1. 运行测试
2. 构建项目
3. 推送到部署目标
4. 验证部署结果
```

**在子 Agent 中运行的研究技能：**

```yaml
---
name: deep-research
description: 深入研究某个话题
context: fork
agent: Explore
---

研究 $ARGUMENTS：
1. 使用 Glob/Grep 查找相关文件
2. 阅读并分析内容
3. 附带文件引用给出总结
```

---

## 四、CLI 命令行参数（终端直接使用）

| 命令 | 说明 |
| ------ | ------ |
| `claude` | 启动交互会话 |
| `claude "query"` | 带初始提示启动会话 |
| `claude -p "query"` | 非交互模式查询并退出（print 模式） |
| `cat file \| claude -p "query"` | 处理管道输入 |
| `claude -c` | 继续最近的会话 |
| `claude -r "<session>" "query"` | 恢复指定会话 |
| `claude update` | 更新到最新版本 |
| `claude auth login` | 登录账户 |
| `claude auth logout` | 退出登录 |
| `claude mcp` | 配置 MCP 服务器 |
| `claude plugin` | 管理插件 |
| `claude agents` | 列出子 Agent |

**常用标志：** `--model`、`--effort`、`--permission-mode`、`--bare`、`--remote`、`--worktree`、`--add-dir`

---

## 五、其他参考文档

- [Claude Code拓展阅读](https://ucn9uf8devd7.feishu.cn/wiki/EF5zw69MXijTpLkrGdycQBQqnCc)
- [省钱秘笈](https://scn3m6p680p5.feishu.cn/docx/HskCdORZ4oOqYuxO8xXcuJb5nsg)
- [入门指南](https://scn3m6p680p5.feishu.cn/docx/KhRQdGaDpoQauDxSruIcAluinrf)
- [实战技巧](https://scn3m6p680p5.feishu.cn/docx/LJYDdU3fooStsmxVwd7cgNCpnNf)
