---
title: 新建一篇文章
author: cotes
date: 2019-08-08 14:10:00 +0800
categories: [博客, Chirpy]
tags: [writing]
render_with_liquid: false
description: 本文介绍在 Chirpy 中创建与发布新文章的完整写作流程。
---

> 本教程将指导你如何在 **Chirpy** 模板中编写一篇文章。即使你之前使用过 Jekyll，也非常值得阅读，因为 Chirpy 的许多功能都依赖于特定变量的正确设置。

> 官方文章翻译版，版本：chirpy-7.4.1
{: .prompt-info }

## 命名与路径

创建一个名为 `YYYY-MM-DD-TITLE.EXTENSION`{: .filepath} 的新文件，并将其放在项目根目录下的 `_posts`{: .filepath} 目录中。
请注意，`EXTENSION`{: .filepath} 只能是 `md`{: .filepath} 或 `markdown`{: .filepath}。

如果你想节省创建文件的时间，建议使用插件 [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose) 来完成这一操作。

## Front Matter

基本上，你需要在文章顶部填写如下所示的 [Front Matter](https://jekyllrb.com/docs/front-matter/) 信息：

```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [TOP_CATEGORY, SUB_CATEGORY]
tags: [TAG]     # 标签名应始终使用小写
---
```

> 文章的 `_layout_` 默认已设置为 `post`，因此不需要在 Front Matter 中额外添加 `layout` 变量。
{: .prompt-tip }

### 日期的时区设置

为了准确记录文章的发布时间，你不仅需要在 `_config.yml`{: .filepath} 中设置 `timezone`，还需要在文章 Front Matter 的 `date` 字段中指定时区。

格式为：`+/-TTTT`，例如：`+0800`。

### 分类与标签

每篇文章的 `categories` 最多支持两个层级，而 `tags` 的数量则不受限制。例如：

```yaml
---
categories: [Animal, Insect]
tags: [bee]
---
```

### 作者信息

通常情况下，文章的作者信息不需要在 Front Matter 中填写，默认会从配置文件中的 `social.name` 和 `social.links` 的第一项中读取。

如果你想自定义作者信息，可以在 `_data/authors.yml` 中添加（如果没有该文件，可以自行创建）：

```yaml
<author_id>:
  name: <full name>
  twitter: <twitter_of_author>
  url: <homepage_of_author>
```

{: file="_data/authors.yml" }

然后在文章中使用 `author`（单个作者）或 `authors`（多个作者）指定：

```yaml
---
author: <author_id>                     # 单作者
# 或
authors: [<author1_id>, <author2_id>]   # 多作者
---
```

需要注意的是，`author` 字段同样可以指定多个作者。

> 从 `_data/authors.yml`{: .filepath} 读取作者信息的好处是，页面中会生成 `twitter:creator` 的 meta 标签，这有助于丰富 [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution)，并提升 SEO 效果。
{: .prompt-info }

### 文章描述

默认情况下，文章开头的内容会被用作首页文章列表、“延伸阅读”区域以及 RSS 中的摘要。

如果你不希望使用自动生成的摘要，可以在 Front Matter 中通过 `description` 字段进行自定义：

```yaml
---
description: 文章的简要说明。
---
```

此外，该描述还会显示在文章页面标题下方。

## 目录（TOC）

默认情况下，文章右侧会显示 **目录（Table of Contents）**。

- 若想全局关闭目录，可在 `_config.yml`{: .filepath} 中将 `toc` 设置为 `false`
- 若只想对某一篇文章关闭目录，可在该文章的 Front Matter 中添加：

```yaml
---
toc: false
---
```

## 评论系统

评论系统的全局配置由 `_config.yml`{: .filepath} 中的 `comments.provider` 决定。一旦设置完成，所有文章都会启用评论功能。

如果你希望为某一篇文章关闭评论，可以在 Front Matter 中添加：

```yaml
---
comments: false
---
```

## 媒体资源

在 **Chirpy** 中，图片、音频和视频统称为媒体资源。

### URL 前缀

当一篇文章中包含大量媒体资源时，可以通过以下两个配置来避免重复书写路径前缀。

- **使用 CDN**

  如果你使用 CDN 托管媒体文件，可以在 `_config.yml`{: .filepath} 中指定 `cdn`：

  ```yaml
  cdn: https://cdn.com
  ```

  {: file='_config.yml' .nolineno }

- **为当前文章指定资源子路径**

  在文章的 Front Matter 中设置 `media_subpath`：

  ```yaml
  ---
  media_subpath: /path/to/media/
  ---
  ```

  {: .nolineno }

最终资源路径将按照以下规则组合：`[site.cdn/][page.media_subpath/]file.ext`

### 图片

#### 图片说明（Caption）

在图片下一行添加斜体文字，即可作为图片说明显示：

```markdown
![img-description](/path/to/image)
_图片说明文字_
```

{: .nolineno}

#### 图片尺寸

为了避免图片加载时导致页面布局抖动，建议为每张图片设置宽高：

```text
![Desktop View](/assets/img/sample/mockup.png){: width="700" height="400" }
```

> 对于 SVG 图片，至少需要指定 `width`，否则无法正常渲染。
{: .prompt-info }

从 **Chirpy v5.0.0** 开始，`width` 和 `height` 支持简写形式：

```markdown
![Desktop View](/assets/img/sample/mockup.png){: w="700" h="400" }
```

{: .nolineno}

#### 图片位置

图片默认居中显示，你也可以通过以下类名控制位置：`normal`、`left`、`right`。

> 一旦指定了图片位置，就不应再添加图片说明。
{: .prompt-warning }

- **普通位置（左对齐）**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .normal }
  ```

  {: .nolineno}

- **左浮动**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .left }
  ```

  {: .nolineno}

- **右浮动**

  ```markdown
  ![Desktop View](/assets/img/sample/mockup.png){: .right }
  ```

  {: .nolineno}

#### 深色 / 浅色模式

可以为深色模式和浅色模式分别准备图片：

```markdown
![Light mode only](/path/to/light-mode.png){: .light }
![Dark mode only](/path/to/dark-mode.png){: .dark }
```

#### 阴影效果

程序窗口截图通常适合加上阴影效果：

```markdown
![Desktop View](/assets/img/sample/mockup.png){: .shadow }
```

{: .nolineno}

#### 文章预览图

如果你想在文章顶部显示一张预览图，图片分辨率应为 **1200 × 630**。
若图片比例不是 `1.91 : 1`，将会被自动裁剪。

配置方式如下：

```yaml
---
image:
  path: /path/to/image
  alt: 图片替代文本
---
```

如果已设置 `media_subpath`，则只需填写图片文件名即可。

你也可以使用简写形式：

```yaml
---
image: /path/to/image
---
```

#### LQIP（低质量占位图）

用于文章预览图：

```yaml
---
image:
  lqip: /path/to/lqip-file # 或 base64
---
```

用于普通图片：

```markdown
![Image description](/path/to/image){: lqip="/path/to/lqip-file" }
```

{: .nolineno }

## 社交媒体嵌入

可以使用以下语法嵌入主流平台的视频或音频：

```liquid
{% include embed/{Platform}.html id='{ID}' %}
```

其中：

- `Platform` 为平台名称的小写形式
- `ID` 为对应资源的 ID

以下表格展示了如何从给定的视频/音频 URL 中获取我们所需的两个参数，并且您还能了解到当前支持的视频平台。

| Video URL                                                                                                                  | Platform   | ID                       |
| -------------------------------------------------------------------------------------------------------------------------- | ---------- | :----------------------- |
| [https://www.**youtube**.com/watch?v=**H-B46URT4mg**](https://www.youtube.com/watch?v=H-B46URT4mg)                         | `youtube`  | `H-B46URT4mg`            |
| [https://www.**twitch**.tv/videos/**1634779211**](https://www.twitch.tv/videos/1634779211)                                 | `twitch`   | `1634779211`             |
| [https://www.**bilibili**.com/video/**BV1Q44y1B7Wf**](https://www.bilibili.com/video/BV1Q44y1B7Wf)                         | `bilibili` | `BV1Q44y1B7Wf`           |
| [https://www.open.**spotify**.com/track/**3OuMIIFP5TxM8tLXMWYPGV**](https://open.spotify.com/track/3OuMIIFP5TxM8tLXMWYPGV) | `spotify`  | `3OuMIIFP5TxM8tLXMWYPGV` |

Spotify 还支持一些额外的参数：

- `compact` —— 以紧凑模式显示播放器
  （示例：`{% include embed/spotify.html id='3OuMIIFP5TxM8tLXMWYPGV' compact=1 %}`）
- `dark` —— 强制使用深色主题
  （示例：`{% include embed/spotify.html id='3OuMIIFP5TxM8tLXMWYPGV' dark=1 %}`）

### 视频文件

如果你希望直接嵌入一个本地或远程的视频文件，可以使用以下语法：

```liquid
{% include embed/video.html src='{URL}' %}
```

其中，`URL` 是视频文件的地址，例如：`/path/to/sample/video.mp4`。

你还可以为嵌入的视频指定额外的属性，以下是支持的完整参数列表：

- `poster='/path/to/poster.png'` —— 视频加载过程中显示的封面图
- `title='Text'` —— 视频标题，显示在视频下方，样式与图片标题一致
- `autoplay=true` —— 视频在可播放时自动开始播放
- `loop=true` —— 视频播放结束后自动从头开始
- `muted=true` —— 视频初始状态为静音
- `types` —— 指定额外的视频格式扩展名，使用 `|` 分隔
  （请确保这些文件与主视频文件位于同一目录）

下面是一个使用了上述所有参数的完整示例：

```liquid
{%
  include embed/video.html
  src='/path/to/video.mp4'
  types='ogg|mov'
  poster='poster.png'
  title='Demo video'
  autoplay=true
  loop=true
  muted=true
%}
```

### 音频文件

如果你希望直接嵌入一个音频文件，可以使用以下语法：

```liquid
{% include embed/audio.html src='{URL}' %}
```

其中，`URL` 是音频文件的地址，例如：`/path/to/audio.mp3`。

同样地，你也可以为嵌入的音频指定额外属性，以下是支持的完整参数列表：

- `title='Text'` —— 音频标题，显示在音频下方，样式与图片标题一致
- `types` —— 指定额外的音频格式扩展名，使用 `|` 分隔
  （请确保这些文件与主音频文件位于同一目录）

示例如下：

```liquid
{%
  include embed/audio.html
  src='/path/to/audio.mp3'
  types='ogg|wav|aac'
  title='Demo audio'
%}
```

## 固定文章（Pinned Posts）

你可以将一篇或多篇文章固定在首页顶部，固定文章会按发布时间倒序排列。

启用方式：

```yaml
---
pin: true
---
```

## 提示块（Prompts）

Chirpy 支持以下提示类型：`tip`、`info`、`warning`、`danger`。

示例：

```md
> 示例提示内容
{: .prompt-info }
```

{: .nolineno }

## 语法说明

### 行内代码

```md
`inline code`
```

### 文件路径高亮

```md
`/path/to/file`{: .filepath}
```

### 代码块

使用三反引号即可创建代码块：

~~~md
```
这是一个纯文本代码块
```
~~~

#### 指定代码语言（Specifying Language）

使用 ````{language}` 的形式可以创建一个**带语法高亮**的代码块，例如：

~~~markdown
```yaml
key: value
```
~~~

> 注意：Jekyll 自带的 `{% highlight %}` 标签 **不兼容** Chirpy 主题，请不要使用。
{: .prompt-danger }

------

#### 行号显示（Line Number）

默认情况下，除了 `plaintext`、`console` 和 `terminal` 这几种语言以外，**所有代码块都会显示行号**。

如果你希望隐藏某个代码块的行号，可以为它添加 `nolineno` 类：

~~~markdown
```shell
echo 'No more line numbers!'
```
{: .nolineno }
~~~

------

#### 指定文件名（Specifying the Filename）

你可能已经注意到，代码块顶部默认显示的是**代码语言名称**。
如果你希望将其替换为**文件名**，可以为代码块添加 `file` 属性：

~~~markdown
```shell
# content
```
{: file="path/to/file" }
~~~

这样，代码块顶部将显示文件路径或文件名，而不是语言类型。

------

#### Liquid 代码（Liquid Codes）

如果你希望在文章中**原样显示 Liquid 代码**（而不是被 Jekyll 执行），需要使用 `{% raw %}` 和 `{% endraw %}` 将其包裹起来：

~~~markdown
{% raw %}
```liquid
{% if product.title contains 'Pack' %}
  This product's title contains the word Pack.
{% endif %}
```
{% endraw %}
~~~

或者，你也可以在文章的 YAML 头信息中添加：

```yaml
render_with_liquid: false
```

> 该方式要求 **Jekyll 4.0 或更高版本**。

------

## 数学公式（Mathematics）

Chirpy 使用 [**MathJax**](https://www.mathjax.org/) 来渲染数学公式。
出于性能考虑，**数学功能默认不会加载**，你需要手动启用。

启用方式是在文章的 YAML 头信息中添加：

```yaml
---
math: true
---
```

------

### 数学公式语法说明

启用数学功能后，你可以通过以下方式插入公式：

- **块级公式（Block math）**
  - 使用 `$$ math $$`
  - `$$` 前后 **必须保留空行**
- **带编号的公式**
  - 使用 `\begin{equation} ... \end{equation}`
  - 使用 `\label{eq:label_name}` 定义编号
  - 使用 `\eqref{eq:label_name}` 在正文中引用
- **行内公式（普通文本中）**
  - 使用 `$$ math $$`
  - 前后 **不需要空行**
- **列表中的行内公式**
  - 使用 `\$$ math $$`（需要转义第一个 `$`）

完整示例如下：

```markdown
<!-- 块级公式，必须保留空行 -->

$$
LaTeX_math_expression
$$

<!-- 带编号的公式，必须保留空行 -->

$$
\begin{equation}
  LaTeX_math_expression
  \label{eq:label_name}
\end{equation}
$$

可以通过 \eqref{eq:label_name} 进行引用。

<!-- 行内公式（普通文本中），不需要空行 -->

"Lorem ipsum dolor sit amet, $$ LaTeX_math_expression $$ consectetur adipiscing elit."

<!-- 列表中的行内公式，需要转义第一个 `$` -->

1. \$$ LaTeX_math_expression $$
2. \$$ LaTeX_math_expression $$
3. \$$ LaTeX_math_expression $$
```

------

> 从 **Chirpy v7.0.0** 开始，**MathJax 的配置选项**已移动到
> `assets/js/data/mathjax.js`{: .filepath } 文件中。
>
> 你可以在该文件中自定义 MathJax 的行为，例如启用额外的 [扩展功能](https://docs.mathjax.org/en/latest/input/tex/extensions/index.html)。
> 如果你使用的是 `chirpy-starter`，请从主题 gem 的安装目录中复制该文件
> （可通过 `bundle info --path jekyll-theme-chirpy` 查看路径），
> 然后放置到你自己仓库的对应位置。
{: .prompt-tip }

------

## Mermaid 图表

[**Mermaid**](https://github.com/mermaid-js/mermaid) 是一个非常强大的图表生成工具，可用于绘制流程图、时序图、甘特图等。

如果你希望在文章中使用 Mermaid，需要在 YAML 头信息中启用它：

```yaml
---
mermaid: true
---
```

启用后，你可以像使用其他 Markdown 代码块一样使用 Mermaid，只需将图表代码包裹在 ````mermaid` 和 ````` 中即可。

------

## 延伸阅读（Learn More）

如果你想深入了解 Jekyll 文章相关的更多细节，请参考官方文档：

👉 [Jekyll Docs：Posts](https://jekyllrb.com/docs/posts/)

