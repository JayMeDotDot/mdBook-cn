# Markdown

mdBook 的[解析器](https://github.com/raphlinus/pulldown-cmark)遵循 [CommonMark](https://commonmark.org/) 规范，并包含下文所述的一些扩展。你可以快速阅读[教程](https://commonmark.org/help/tutorial/)，或者[实时试用](https://spec.commonmark.org/dingus/) CommonMark。完整的 Markdown 概述超出了本文档的范围，下面只是对一些基础知识的高层次介绍。想要更深入的了解，请查阅 [Markdown 指南](https://www.markdownguide.org)。

## 文本与段落

文本的渲染效果是相对可预测的：

```markdown
这里有一行文本。

这是新的一行。
```

渲染效果如你所预期：

这里有一行文本。

这是新的一行。

## 标题

标题使用 `#` 标记，并且应该独占一行。`#` 越多，标题层级越小：

```markdown
### 一个标题

一些文本。

#### 一个更小的标题

更多文本。
```

### 一个标题

一些文本。

#### 一个更小的标题

更多文本。

## 列表

列表可以是无序列表，也可以是有序列表。有序列表会自动排序：

```markdown
* 牛奶
* 鸡蛋
* 黄油

1. 胡萝卜
1. 芹菜
1. 小萝卜
```

* 牛奶
* 鸡蛋
* 黄油

1. 胡萝卜
1. 芹菜
1. 小萝卜

## 链接

链接到 URL 或本地文件很简单：

```markdown
使用 [mdBook](https://github.com/rust-lang/mdBook)。

阅读有关 [mdBook](mdbook.md) 的内容。

现在还有一个 [一个 mdBook 链接][an mdBook link]，与上面的不同，它不是内联的。

一个裸 URL：<https://www.rust-lang.org>。

[一个 mdBook 链接][an mdBook link]: https://github.com/rust-lang/mdBook
```

使用 [mdBook](https://github.com/rust-lang/mdBook)。

阅读有关 [mdBook](mdbook.md) 的内容。

现在还有一个 [一个 mdBook 链接][an mdBook link]，与上面的不同，它不是内联的。

一个裸 URL：<https://www.rust-lang.org>。

[an mdBook link]: https://github.com/rust-lang/mdBook
----

以 `.md` 结尾的相对链接会被转换为 `.html` 扩展名。建议尽可能使用 `.md` 链接。这样在 mdBook 之外查看 Markdown 文件时会很有用，例如在会自动渲染 Markdown 的 GitHub 或 GitLab 上。

指向 `README.md` 的链接会被转换为 `index.html`。这样做是因为 GitHub 等一些服务会自动渲染 README 文件，而 Web 服务器通常期望根文件名为 `index.html`。

可以使用 `#` 片段链接到各个标题。例如，`mdbook.md#text-and-paragraphs` 会链接到上文的 [文本与段落](#文本与段落)一节。ID 是通过对标题进行变换而生成的，例如转换为小写、把空格替换为连字符。你可以点击任意标题，然后查看浏览器中的 URL，看看片段长什么样。

## 图片

插入图片只需像上文 _Links_（链接）一节那样放入指向图片的链接即可。下面的 markdown引用了与本文件同级的 `images` 目录中的 Rust logo SVG 图片：

```markdown
![Rust 标志](images/rust-logo-blk.svg)
```

用 mdBook 构建时会生成如下 HTML：

```html
<p><img src="images/rust-logo-blk.svg" alt="Rust 标志" /></p>
```

当然，它会像这样显示图片：

![Rust 标志](images/rust-logo-blk.svg)

## 扩展

在标准 CommonMark 规范之外，mdBook 还提供了若干扩展。

### 删除线

将文本两侧各包上一个或两个波浪号（~）字符，即可让文本渲染成中间带一条水平线的样子：

```text
这是一个 ~~删除线文本~~ 的示例。
```

这个示例会渲染为：

> 这是一个 ~~删除线文本~~ 的示例。

这遵循 [GitHub 删除线扩展][strikethrough]。

### 脚注

脚注会在文本中生成一个带编号的小链接，点击后即可跳转到位于条目底部的脚注文本。脚注标签的写法与链接引用类似，只是前面多了一个脱字符（^）。脚注文本的写法则与链接引用定义类似，文本跟在标签后面。例如：

```text
这是一个脚注的示例[^note]。

[^note]: 这段文本是脚注的内容，它会被
    渲染到页面的底部。
```

这个示例会渲染为：

> 这是一个脚注的示例[^note]。
>
> [^note]: 这段文本是脚注的内容，它会被
>     渲染到页面的底部。

脚注会根据书写的先后顺序自动编号。

### 表格

表格可以用竖线和横线画出行与列。它们会被转换成与之形状对应的 HTML 表格。例如：

```text
| 表头1 | 表头2 |
|-------|-------|
| abc   | def   |
```

这个示例会渲染成类似这样：

| 表头1 | 表头2 |
|-------|-------|
| abc   | def   |

关于所支持语法的确切细节，请参阅 [GitHub 表格扩展][tables] 的规范。

### 任务列表

任务列表可以用作记录已完成事项的核对清单。例如：

```md
- [x] 已完成的任务
- [ ] 未完成的任务
```

它会渲染为：

> - [x] 已完成的任务
> - [ ] 未完成的任务

更多细节请参阅[任务列表扩展][task list extension]的规范。

### 智能标点

一些 ASCII 标点序列会被自动转换成精美的 Unicode 字符：

| ASCII 序列 | Unicode |
|----------------|---------|
| `--`           | –       |
| `---`          | —       |
| `...`          | …       |
| `"`            | “ 或 ”，视上下文而定 |
| `'`            | ‘ 或 ’，视上下文而定 |

这样就无需手动输入那些 Unicode 字符了！

此功能默认启用。要禁用它，请参阅 [`output.html.smart-punctuation`] 配置项。

[strikethrough]: https://github.github.com/gfm/#strikethrough-extension-
[tables]: https://github.github.com/gfm/#tables-extension-
[task list extension]: https://github.github.com/gfm/#task-list-items-extension-
[`output.html.smart-punctuation`]: configuration/renderers.md#html-渲染器选项

### 标题属性

标题可以有自定义的 HTML ID 和类（class）。这样即使修改了标题文本，也能保持同一个 ID；还可以在标题上添加多个类。

示例：

```md
# 示例标题 { #first .class1 .class2 }
```

这会生成一个一级标题，内容为 `示例标题`，ID 为 `first`，类为 `class1` 和 `class2`。注意，各属性之间应以空格分隔。

更多信息请参阅[标题属性规范页面](https://github.com/raphlinus/pulldown-cmark/blob/master/pulldown-cmark/specs/heading_attrs.txt)。

### 定义列表

定义列表可用于诸如术语表条目之类的场景。术语单独占一行，后面跟一个或多个定义。每个定义必须以 `:` 开头（前面允许 0-2 个空格）。

示例：

```md
术语 A
  : 这是术语 A 的定义。文本
    可以跨越多行。

术语 B
  : 这是术语 B 的定义。
  : 这个术语有不止一个定义。
```

它会渲染为：

术语 A
  : 这是术语 A 的定义。文本
    可以跨越多行。

术语 B
  : 这是术语 B 的定义。
  : 这个术语有不止一个定义。

术语和标题一样可以点击，点击后浏览器的 URL 会直接指向该术语。

关于语法的具体细节，请参阅[定义列表规范](https://github.com/pulldown-cmark/pulldown-cmark/blob/HEAD/pulldown-cmark/specs/definition_lists.txt)。关于如何编写术语表，请参阅 [Wikipedia 术语表指南](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Glossaries#General_guidelines_for_writing_glossaries)。

此功能默认启用。要禁用它，请参阅 [`output.html.definition-lists`] 配置项。

[`output.html.definition-lists`]: configuration/renderers.md#html-渲染器选项

### 提示块

提示块（admonition）是一种特殊的标注（callout），用来突出重要信息。它的写法是一个块引用，并在第一行加上一个特殊标记。

```md
> [!NOTE]
> 常规信息或补充说明。

> [!TIP]
> 有用的建议或最佳实践。

> [!IMPORTANT]
> 不应错过的关键信息。

> [!WARNING]
> 突出潜在风险的重要提示。

> [!CAUTION]
> 需要谨慎对待的潜在问题。
```

它们会渲染为：

> [!NOTE]
> 常规信息或补充说明。

> [!TIP]
> 有用的建议或最佳实践。

> [!IMPORTANT]
> 不应错过的关键信息。

> [!WARNING]
> 突出潜在风险的重要提示。

> [!CAUTION]
> 需要谨慎对待的潜在问题。

此功能默认启用。要禁用它，请参阅 [`output.html.admonitions`] 配置项。

[`output.html.admonitions`]: configuration/renderers.md#html-渲染器选项

## 缩放

章节内容中的所有图片都带有“放大”（zoom-in）功能：点击图片可以放大，再次点击则缩小复原。也可以用键盘将焦点移到图片上，再按空格键来放大和缩小。
