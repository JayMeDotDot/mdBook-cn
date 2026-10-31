# 通用配置

你可以在 ***book.toml*** 文件中配置书籍的各项参数。

下面是一个 ***book.toml*** 文件可能的样子：

```toml
[book]
title = "Example book"
authors = ["John Doe"]
description = "The example book covers examples."

[rust]
edition = "2018"

[build]
build-dir = "my-example-book"
create-missing = false

[preprocessor.index]

[preprocessor.links]

[output.html]
additional-css = ["custom.css"]

[output.html.search]
limit-results = 15
```

## 支持的配置选项

需要注意的是，配置中指定的 **任何** 相对路径，始终都是相对于配置文件所在的那本书的根目录来解析的。

### 通用元数据

这里是关于你这本书的一般信息。

- **title:** 书籍的标题
- **authors:** 书籍的作者（可多名）
- **description:** 书籍的描述，会作为元信息添加到每一页 HTML 的 `<head>` 中
- **src:** 默认情况下，源目录是根文件夹下名为 `src` 的目录。但可以通过配置文件中的 `src` 键来修改
- **language:** 书籍的主要语言，例如会用作语言属性 `<html lang="en">`。它也用于推断书中文本的方向（RTL，从右到左；LTR，从左到右）
- **text-direction**：书中文本的方向：从左到右（LTR）或从右到左（RTL）。可选值：`ltr`、`rtl`。未指定时，会根据书籍的 `language` 属性来推断文本方向。
- **logo:** 显示在导航栏顶部的 logo 的路径，相对于 `<book_root>/<src>/`。

**book.toml**

```toml
[book]
title = "Example book"
authors = ["John Doe", "Jane Doe"]
description = "The example book covers examples."
src = "my-src"  # 将会在 `root/my-src` 路径下查找源文件，而不是 `root/src`
language = "en"
text-direction = "ltr"
logo = "static/logo.svg"  # logo 文件路径为 `root/my-src/static/logo.svg`
```

### Rust 选项

与 Rust 语言相关的选项，涉及测试运行和 playground 集成。

```toml
[rust]
edition = "2015"   # 代码块默认使用的语言版本
```

- **edition**: 代码片段默认使用的 Rust edition。默认值为 `"2015"`。可以通过 `edition2015`、`edition2018`、`edition2021` 或 `edition2024` 标注来单独控制各个代码块，例如：

  ~~~text
  ```rust,edition2015
  // This only works in 2015.
  let try = true;
  ```
  ~~~

### 构建选项

这一部分控制书籍的构建过程。

```toml
[build]
build-dir = "book"                # 输出内容存放的目录
create-missing = true             # 是否创建缺失的页面
use-default-preprocessors = true  # 使用默认预处理器
extra-watch-dirs = []             # 需要监视以触发构建的目录
```

- **build-dir:** 存放渲染结果的目录。默认是书籍根目录下的 `book/`。
  可以通过 `--dest-dir` 命令行选项覆盖。
- **create-missing:** 默认情况下，`SUMMARY.md` 中指定的任何缺失文件都会在构建书籍时被创建（即 `create-missing = true`）。若将其设为 `false`，则当有任何文件不存在时，构建过程会以错误退出。
- **use-default-preprocessors:** 将此选项设为 `false` 可禁用默认预处理器（`links` 和 `index`）。

  如果你通过相应的配置表声明了相同和/或其他预处理器，它们将代替默认预处理器运行。

  - 澄清一下：在没有任何预处理器配置的情况下，默认会运行 `links` 和 `index`。
  - 设置 `use-default-preprocessors = false` 会禁用这些默认预处理器的运行。
  - 例如，添加 `[preprocessor.links]` 就能确保无论 `use-default-preprocessors` 取何值，`links` 都会运行。
- **extra-watch-dirs**: 在 `watch` 和 `serve` 命令中会被监视的目录路径列表。这些目录下的文件发生变化时会触发重新构建。
  当你的书籍依赖于 `src` 目录之外的文件时，这个选项很有用。
