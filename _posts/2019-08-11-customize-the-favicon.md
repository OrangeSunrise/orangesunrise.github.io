---
title: 个性化网站图标
author: cotes
date: 2019-08-11 00:34:00 +0800
categories: [博客, Chirpy]
tags: [favicon]
description: 本文讲解如何生成并替换 Chirpy 站点的 favicon 图标资源。
---

Chirpy主题的favicon文件存放在目录assets/img/favicons/中。你可以将它们替换为你自己的网站图标。下面的内容将一步步指导你如何创建并替换默认的favicon。

> 官方文章翻译版，版本：chirpy-7.4.1
{: .prompt-info }

<!-- markdownlint-disable MD013 MD033 -->
[**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/)主题的
[**favicon**](https://www.favicon-generator.org/about/) 文件存放在目录  `assets/img/favicons/` 中。
你可以将它们替换为你自己的网站图标。
下面的内容将一步步指导你如何创建并替换默认的 favicon。
<!-- markdownlint-disable MD013 MD033 -->

## 生成 favicon

<!-- markdownlint-disable MD013 MD033 -->
准备一张**正方形图片**（PNG、JPG 或 SVG 格式均可），尺寸建议为 **512×512 或更大**。
然后访问在线工具 [**Real Favicon Generator**](https://realfavicongenerator.net/)，
点击按钮 <kbd>Pick your favicon image</kbd> 上传你的图片文件。

在下一步中，网页会展示 favicon 在各种使用场景下的效果。
你可以保持默认配置，直接滚动到页面底部，点击 <kbd>Next →</kbd> 按钮来生成 favicon。
<!-- markdownlint-enable MD013 MD033 -->

## 下载并替换

下载生成好的压缩包，解压后，**删除以下文件**：

- `site.webmanifest`

然后将剩余的图片文件（`.PNG`、`.ICO` 和 `.SVG`）复制到你 Jekyll 网站中的
`assets/img/favicons/` 目录，
用它们**覆盖原有的文件**。
如果你的 Jekyll 站点中还不存在该目录，可以手动创建一个。

下表可以帮助你理解 favicon 文件的替换规则：

| 文件 | 来自在线工具 | Chirpy 自带 |
| --- | --- | --- |
| `*.PNG` | ✓ | ✗ |
| `*.ICO` | ✓ | ✗ |
| `*.SVG` | ✓ | ✗ |

> ✓ 表示保留，✗ 表示删除。
{: .prompt-info }

下一次你构建站点时，网站的 favicon 就会被替换为你自定义的版本。
