---
layout: post
title: 在 Chirpy 主题中启用 Giscus 评论系统
date: 2025-12-21 16:24 +0800
author: cerberus
categories: [博客, 教程]
tags: [chirpy, giscus, 评论系统, GitHub Pages]
---

本文将完整介绍：如何在 Chirpy 主题博客中启用 Giscus 评论系统。

---

## 前言

**Chirpy** 原生支持多种评论系统，其中 **Giscus** 是目前最推荐的一种方案：

- 基于 **GitHub Discussions**
- 无广告、无第三方账号
- 支持 Markdown、表情 Reaction
- 与 GitHub Pages 天然集成

本文将完整介绍：**如何在 Chirpy 主题博客中启用 Giscus 评论系统**。

## 一、前置条件（很重要）

在开始之前，请确认：

1. ✅ 你的博客是 **GitHub Pages**
    
2. ✅ 博客仓库是 **public**
    
    - `OrangeSunrise/orangesunrise.github.io`
        
3. ✅ 你使用的是 **Chirpy 主题**
    
4. ✅ 评论希望基于 **GitHub Discussions**
    

如果以上都满足，继续👇

---

## 二、开启 GitHub Discussions

进入你的博客仓库：

👉 `https://github.com/OrangeSunrise/orangesunrise.github.io`

1. 点击 **Settings**
    
2. 滚动到 **Features**
    
3. 勾选 ✅ **Discussions**
    
4. 点击 **Set up discussions**
    
5. 使用默认分类即可（之后我们会用到）
    

---

## 三、在 Giscus 官网生成配置

打开 👉 **[https://giscus.app/](https://giscus.app/)**

按顺序填写：

### 1️⃣ Repository

```
OrangeSunrise/orangesunrise.github.io
```

### 2️⃣ Discussion 分类

选择一个分类，比如：

```
General
```

### 3️⃣ 映射方式（推荐）

```
Discussion ↔ Page pathname
```

### 4️⃣ 语言

```
Chinese (Simplified)
```

### 5️⃣ 启用 Reaction（推荐）

✔ Enable reactions for comments

---

## 四、复制关键参数（非常关键）

在 giscus.app 页面底部，你会看到一段 `<script>`，从中**只需要下面这些字段**：

```html
data-repo="OrangeSunrise/orangesunrise.github.io"
data-repo-id="R_xxxxxxxxxx"
data-category="General"
data-category-id="DIC_xxxxxxxxxx"
```

---

## 五、填写到 Chirpy 的 `_config.yml`

打开你博客根目录的 `_config.yml`，找到你贴出来的这段，**这样填写**：

```yml
comments:
  provider: giscus

  giscus:
    repo: OrangeSunrise/orangesunrise.github.io
    repo_id: R_xxxxxxxxxx
    category: General
    category_id: DIC_xxxxxxxxxx
    mapping: pathname
    strict: 0
    input_position: bottom
    lang: zh-CN
    reactions_enabled: 1
```
    
> - `repo_id` / `category_id` **必须和 giscus.app 给的一模一样**   
> - 不能加引号    
> - 缩进必须是 **2 个空格**
> - `repo`替换为你自己的
{: .prompt-warning }

---

## 六、提交并部署

```bash
git add _config.yml
git commit -m "feat: enable giscus comments"
git push
```

等待 GitHub Actions 构建完成（一般 1–3 分钟）

---

## 七、验证是否成功

1. 打开任意一篇博客文章
    
2. 滚动到文章底部
    
3. 你应该能看到：
    
    - 💬 评论框
        
    - 👍 GitHub 表情反应
        
    - “Sign in with GitHub”
        

如果看不到，常见原因：

- Discussions 没开
    
- repo / repo_id 填错
    
- 仓库不是 public
    