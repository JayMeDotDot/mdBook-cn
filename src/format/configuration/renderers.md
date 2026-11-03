# 配置渲染器

渲染器（也称为"后端"）负责生成书籍的输出内容。

以下是内置的后端：

* [`html`](#html-渲染器选项) --- 将书籍渲染为 HTML。
  如果 `book.toml` 中没有定义其他 `[output]` 表，则默认启用它。
* [`markdown`](#markdown-渲染器) --- 在运行预处理器之后将书籍以 markdown 格式输出。这对于调试预处理器很有用。

社区已经开发了许多后端。可用后端的列表参见 [第三方插件][Third Party Plugins] wiki 页面。

关于如何创建新后端，参见 [后端][Backends for Developers] 一章。

[Third Party Plugins]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins#backends
[Backends for Developers]: ../../for_developers/backends.md

## Output 表

在 `book.toml` 中加入一个以后端名称命名的 `output` 表即可添加后端。
例如，如果你有一个名为 `mdbook-wordcount` 的后端，可以这样引入它：

```toml
[output.wordcount]
```

有了这个表，mdBook 就会执行 `mdbook-wordcount` 后端。

这个表还可以包含该后端特有的额外键值对。
例如，如果我们的示例后端需要一些额外的配置选项：

```toml
[output.wordcount]
ignores = ["Example Chapter"]
```

只要定义了任何 `[output]` 表，`html` 后端就不再默认启用。
如果想继续使用 `html` 后端，只需把它也写进 `book.toml` 文件即可。
例如：

```toml
[book]
title = "My Awesome Book"

[output.wordcount]

[output.html]
```

如果包含多个 `output` 表，输出目录的布局方式会有所不同。
如果只有一个后端，它的输出会直接放在 `book` 目录中（可通过 [`build.build-dir`] 覆盖该位置）。
如果有多个后端，每个后端的输出会放在 `book` 下各自独立的目录中。
例如，上面的配置会生成 `book/html` 和 `book/wordcount` 目录。

[`build.build-dir`]: general.md#构建选项

### 自定义后端命令

默认情况下，当你在 `book.toml` 文件中添加 `[output.foo]` 表时，
`mdbook` 会尝试调用 `mdbook-foo` 可执行文件。
如果你想使用不同的程序名或传入命令行参数，
可以通过添加 `command` 字段来覆盖这一行为。

```toml
[output.random]
command = "python random.py"
```

### 可选后端

如果启用的后端尚未安装，默认行为是抛出一个错误。
可以通过将该后端标记为可选来改变这一行为：

```toml
[output.wordcount]
optional = true
```

这会将错误降级为警告。

## HTML 渲染器选项

HTML 渲染器有许多选项，详见下文。
这些选项应在 `book.toml` 文件的 `[output.html]` 表中指定。

```toml
# 包含所有 output 选项的示例 book.toml 文件。
[book]
title = "Example book"
authors = ["John Doe", "Jane Doe"]
description = "The example book covers examples."

[output.html]
theme = "my-theme"
default-theme = "light"
preferred-dark-theme = "navy"
smart-punctuation = true
definition-lists = true
admonitions = true
mathjax-support = false
additional-css = ["custom.css", "custom2.css"]
additional-js = ["custom.js"]
no-section-label = false
git-repository-url = "https://github.com/rust-lang/mdBook"
git-repository-icon = "fab-github"
edit-url-template = "https://github.com/rust-lang/mdBook/edit/main/guide/{path}"
site-url = "/example-book/"
cname = "myproject.rs"
input-404 = "not-found.md"
sidebar-header-nav = true
zoomable-images = true
```

可用的配置选项如下：

- **theme:** mdBook 自带一个默认主题以及它所需的全部资源文件。但如果设置了此选项，mdBook 会用指定文件夹中找到的文件选择性地覆盖主题文件。
- **default-theme:** 在 'Change Theme' 下拉菜单中默认选中的内置主题。可选值为 `light`、`rust`、`coal`、`navy` 和 `ayu`。默认值为 `light`。
- **preferred-dark-theme:** 当浏览器通过
  [`prefers-color-scheme`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)CSS 媒体查询请求网站的深色版本时使用的内置主题。可选值为 `light`、`rust`、`coal`、`navy` 和 `ayu`。默认值为 `navy`。
- **smart-punctuation:** 将引号转换为弯引号，`...` 转换为 `…`，`--` 转换为 en-dash（短破折号），`---` 转换为 em-dash（长破折号）。参见 [智能标点](../markdown.md#智能标点)。默认值为 `true`。
- **definition-lists:** 启用[定义列表](../markdown.md#定义列表)。默认值为 `true`。
- **admonitions:** 启用 [admonitions（提示块）](../markdown.md#提示块)。默认值为 `true`。
- **mathjax-support:** 添加对 [MathJax](../mathjax.md) 的支持。默认值为 `false`。
- **additional-css:** 如果你只想略微调整书籍的外观而不想覆盖整个样式，可以指定一组样式表，它们会在默认样式表之后加载，你可以在其中对样式进行精细的修改。
- **additional-js:** 如果你需要在书籍中添加一些行为而不移除现有行为，可以指定一组 JavaScript 文件，它们会与默认的 JavaScript 一起加载。
- **no-section-label:** mdBook 默认会在目录栏中添加数字形式的章节编号，例如 "1."、"2.1"。将此选项设为 true 可禁用这些编号。默认值为 `false`。
- **git-repository-url:** 书籍的 git 仓库地址。如果提供了该地址，书籍的菜单栏中会输出一个图标链接。
- **git-repository-icon:** git 仓库链接使用的 Font Awesome 图标类名。默认值为 `fab-github`，其外观为 <i class="fab fa-github"></i>。如果你使用的不是 GitHub，可以考虑 `fas-code-fork`，其外观为 <i class="fas fa-code-fork"></i>。字符串的开头应为 `fa-`（常规图标）、`fas-`（实心图标）或 `fab-`（品牌图标）。可用图标参见[免费图标集](https://fontawesome.com/v6/search)。
- **edit-url-template:** 编辑 URL 模板。提供后，会显示一个"建议编辑"按钮（外观为 <i class="fas fa-pencil"></i>），可直接跳转到编辑当前浏览的页面。例如，对于 GitHub 项目，将其设置为 `https://github.com/<owner>/<repo>/edit/<branch>/{path}`；对于 Bitbucket 项目，将其设置为 `https://bitbucket.org/<owner>/<repo>/src/<branch>/{path}?mode=edit`，其中 {path} 会被替换为文件在仓库中的完整路径。
- **input-404:** 用于处理缺失文件的 markdown 文件的名称。
  对应的输出文件名相同，只是扩展名替换为 `html`。
  默认值为 `404.md`。
- **site-url:** 书籍托管的地址。此项用于确保导航链接以及 404 文件中的脚本/css 导入能正常工作，即使访问的是子目录中的 URL 也如此。默认值为 `/`。如果设置了 `site-url`，请确保资源的链接使用相对于文档的路径，即不应以 `/` 开头。
- **canonical-site-url:** 设置书籍的规范 URL（canonical URL），搜索引擎会用它来确定内容的主 URL。当你的站点部署在多个 URL 上时可以使用此选项。例如，当你有针对一系列版本的站点部署时，可以将它们全部指向最新版本的 URL。如果不设置，你的内容可能因内容重复而受到搜索引擎的惩罚，访问者也可能被引导到书籍的过时版本。
- **cname:** 书籍托管的 DNS 子域或顶级域名（apex domain）。
  该字符串会被写入站点根目录下名为 CNAME 的文件中，这是 GitHub Pages 的要求（参见 [*管理 GitHub Pages 站点的自定义域名*][custom domain]）。
- **hash-files:** 在静态资源的文件名中加入文件内容的加密"指纹"，这样当文件内容发生变化时，文件名也会随之变化。例如，`css/chrome.css` 可能变成 `css/chrome-9b8f428e.css`。章节 HTML 文件不会被重命名。静态 CSS 和 JS 文件可以使用 `{{ resource "filename" }}` 指令相互引用。默认值为 `true`。
- **sidebar-header-nav:** 如果为 `true`，侧边栏将包含当前页面各标题的导航。默认值为 `true`。
- **zoomable-images:** 如果为 `true`，点击图片时会打开一个模态框，以放大的视图显示图片。默认值为 `true`。

[custom domain]: https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site

### `[output.html.print]`

`[output.html.print]` 表提供了控制可打印输出的选项。默认情况下，mdBook 会在书籍右上角包含一个图标（外观为 <i class="fas fa-print"></i>），点击它可以将整本书作为单个页面打印。

```toml
[output.html.print]
enable = true    # 启用对可打印输出的支持
page-break = true # 在每章之后插入分页符
```

- **enable:** 启用打印支持。设为 `false` 时，所有打印支持都不会被渲染。默认值为 `true`。
- **page-break:** 在章节之间插入分页符。默认值为 `true`。

### `[output.html.fold]`

`[output.html.fold]` 表提供了控制导航侧边栏中章节列表折叠行为的选项。

```toml
[output.html.fold]
enable = false    # 是否启用章节折叠
level = 0         # 开始折叠的深度
```

- **enable:** 启用章节折叠。关闭时，所有折叠都处于展开状态。默认值为 `false`。
- **level:** 数值越大，展开的折叠区域越多。当 level 为 0 时，所有折叠都处于关闭状态。默认值为 `0`。

### `[output.html.playground]`

`[output.html.playground]` 表提供了控制 Rust 示例代码块及其与 [Rust Playground] 集成的选项。

[Rust Playground]: https://play.rust-lang.org/

```toml
[output.html.playground]
editable = false         # 允许编辑源代码
copyable = true          # 显示用于复制代码片段的复制按钮
copy-js = true           # 引入代码编辑器所需的 JavaScript
line-numbers = false     # 在可编辑代码上显示行号
runnable = true          # 为 rust 代码显示运行按钮
```

- **editable:** 允许编辑源代码。默认值为 `false`。
- **copyable:** 在代码片段上显示复制按钮。默认值为 `true`。
- **copy-js:** 将代码编辑器所需的 JavaScript 文件复制到输出目录。默认值为 `true`。
- **line-numbers:** 在可编辑的代码段上显示行号。需要 `editable` 和 `copy-js` 都为 `true`。默认值为 `false`。
- **runnable:** 为 rust 代码片段显示运行按钮。将其改为 `false` 会全局禁用"在 Playground 中运行"功能。默认值为 `true`。

### `[output.html.code]`

`[output.html.code]` 表提供了控制代码块的选项。

```toml
[output.html.code]
# 每种语言对应一个前缀字符串（一个或多个字符）。
# 任何以 空白字符+前缀 开头的行都会被隐藏。
hidelines = { python = "~" }
```

- **hidelines:** 一个表，用于定义各语言中[隐藏代码行](../mdbook.md#隐藏代码行)的工作方式。键是语言，值是一个字符串，以该前缀开头的代码行将被隐藏。

### `[output.html.search]`

`[output.html.search]` 表提供了控制内置文本[搜索][search]功能的选项。mdBook 必须在编译时启用 `search` feature（默认开启）。

[search]: ../../guide/reading.md#搜索

```toml
[output.html.search]
enable = true            # 启用搜索功能
limit-results = 30       # 搜索结果的最大数量
teaser-word-count = 30   # 搜索结果摘要所使用的单词数
use-boolean-and = true   # 多个搜索词必须全部匹配
boost-title = 2          # 搜索词出现在标题中时的排序加权系数
boost-hierarchy = 1      # 搜索词出现在页面名称中时的排序加权系数
boost-paragraph = 1      # 搜索词出现在正文中的排序加权系数
expand = true            # 部分单词将匹配更长的词
heading-split-level = 3  # 将搜索结果链接到标题层级
copy-js = true           # 引入搜索所需的 Javascript 代码
```

- **enable:** 启用搜索功能。默认值为 `true`。
- **limit-results:** 搜索结果的最大数量。默认值为 `30`。
- **teaser-word-count:** 搜索结果摘要所使用的单词数。默认值为 `30`。
- **use-boolean-and:** 定义多个搜索词之间的逻辑关系。若为 true，则所有搜索词都必须出现在每个结果中。默认值为 `false`。
- **boost-title:** 当搜索词出现在标题中时，搜索结果得分的加权系数。默认值为 `2`。
- **boost-hierarchy:** 当搜索词出现在层级中时，搜索结果得分的加权系数。层级包含所有父文档的标题以及所有父级标题。默认值为 `1`。
- **boost-paragraph:** 当搜索词出现在正文中时，搜索结果得分的加权系数。默认值为 `1`。
- **expand:** 若为 true，搜索将匹配更长的词，例如搜索 `micro` 应匹配 `microwave`。默认值为 `true`。
- **heading-split-level:** 搜索结果将链接到文档中包含该结果的章节。文档会按不高于此级别的标题拆分成多个章节。默认值为 `3`。（`### This is a level 3 heading`）
- **copy-js:** 将搜索实现所需的 JavaScript 文件复制到输出目录。默认值为 `true`。

#### `[output.html.search.chapter]`

[`output.html.search.chapter`] 表允许按章节或目录修改搜索设置。每个键是章节源文件或目录的路径，值是要应用到该路径的设置表。设置会递归合并，更具体的路径优先。

```toml
[output.html.search.chapter]
# 禁用 `appendix` 目录中所有章节的搜索索引。
"appendix" = { enable = false }
# 仅为这一个附录章节启用搜索索引。
"appendix/glossary.md" = { enable = true }
```

- **enable:** 启用或禁用给定章节的搜索索引。默认值为 `true`。此项不会覆盖整体的 `output.html.search.enable` 设置；后者必须为 `true`，任何搜索功能才会启用。禁用章节索引时请谨慎，因为当用户搜索某些词并期望能找到它们时，这可能会造成困惑。仅应在特殊情况下使用，即保留该章节在索引中会导致搜索结果质量出现问题时。

### `[output.html.redirect]`

`[output.html.redirect]` 表提供了一种添加重定向的方式。当你移动、重命名或删除某个页面时，它很有用，可以确保指向旧 URL 的链接跳转到新的位置。

```toml
[output.html.redirect]
"/appendices/bibliography.html" = "https://rustc-dev-guide.rust-lang.org/appendix/bibliography.html"
"/other-installation-methods.html" = "../infra/other-installation-methods.html"

# 片段重定向同样有效。
"/some-existing-page.html#old-fragment" = "some-existing-page.html#new-fragment"

# 片段重定向对已删除的页面同样有效。
"/old-page.html" = "new-page.html"
"/old-page.html#old-fragment" = "new-page.html#new-fragment"
```

该表包含键值对，其中键是需要创建重定向文件的位置，以构建目录为基准的绝对路径表示（例如 `/appendices/bibliography.html`）。值可以是浏览器应跳转到的任何有效 URI（例如 `https://rust-lang.org/`、`/overview.html` 或 `../bibliography.html`）。

这会生成一个 HTML 页面，它会自动重定向到指定的位置。

当指定了片段（fragment）重定向时，页面必须使用 JavaScript 来重定向到正确的位置。如果你重命名或移动了某个章节标题，这会很有用。片段重定向对已存在的页面和已删除的页面都有效。

## Markdown 渲染器

Markdown 渲染器会先运行预处理器，然后输出所得的 Markdown。这主要用于调试预处理器，尤其是配合 `mdbook test` 使用，以查看 `mdbook` 传给 `rustdoc` 的 Markdown。

Markdown 渲染器随 `mdbook` 一同提供，但默认是禁用的。按如下方式在 `book.toml` 中添加一个空表即可启用它：

```toml
[output.markdown]
```

目前 Markdown 渲染器没有任何配置选项；只有启用或禁用之分。

关于如何指定在 Markdown 渲染器之前运行哪些预处理器，参见[预处理器文档](preprocessors.md)。
