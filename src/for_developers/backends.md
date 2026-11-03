# 其他后端

所谓“后端”（backend），其实就是 `mdbook` 在书籍渲染过程中调用的一个程序。书籍的 JSON 表示以及配置信息会通过 `stdin` 传给该程序。后端收到这些信息之后，就可以随心所欲地做任何事。

关于使用后端的更多信息，请参阅[配置渲染器](../format/configuration/renderers.md)。

社区已经开发了多个后端。可用后端的列表请参见 [第三方插件][Third Party Plugins] wiki 页面。

## 准备工作

本页将通过一个简单的字数统计程序，带你一步步创建自己的替代后端。虽然示例将用 Rust 编写，但用 Python 或 Ruby 之类的语言来完成也完全没问题。

首先，你需要创建一个新的二进制程序，并添加 `mdbook-renderer` 作为依赖。

```shell
$ cargo new --bin mdbook-wordcount
$ cd mdbook-wordcount
$ cargo add mdbook-renderer
```

当我们的 `mdbook-wordcount` 插件被调用时，`mdbook` 会通过插件的 `stdin` 将 [`RenderContext`] 的 JSON 版本发送给它。为了方便，还提供了一个 [`RenderContext::from_json()`] 构造函数，用于加载 `RenderContext`。

这就是我们的后端加载书籍所需的全部样板代码。

```rust
// src/main.rs
use std::io;
use mdbook_renderer::RenderContext;

fn main() {
    let mut stdin = io::stdin();
    let ctx = RenderContext::from_json(&mut stdin).unwrap();
}
```

> **注意：** `RenderContext` 包含一个 `version` 字段。后端可以据此判断自己与调用它的 `mdbook` 版本是否兼容。这个 `version` 直接取自 `mdbook` 的 `Cargo.toml` 中对应的字段。
>
> 建议后端使用 [`semver`] crate 来检查该字段，并在可能存在兼容性问题时发出警告。

## 检查书籍

现在我们的后端已经拿到了书籍的副本，让我们来数一数每一章有多少个单词吧！

由于 `RenderContext` 包含一个 [`Book`] 字段（`book`），而 `Book` 提供了 [`Book::iter()`] 方法用于迭代书中的所有条目，所以这一步和第一步一样简单。

```rust

fn main() {
    let mut stdin = io::stdin();
    let ctx = RenderContext::from_json(&mut stdin).unwrap();

    for item in ctx.book.iter() {
        if let BookItem::Chapter(ref ch) = *item {
            let num_words = count_words(ch);
            println!("{}: {}", ch.name, num_words);
        }
    }
}

fn count_words(ch: &Chapter) -> usize {
    ch.content.split_whitespace().count()
}
```

## 启用后端

基本功能已经跑通了，接下来我们想真正用上它。首先，安装这个程序。

```shell
$ cargo install --path .
```

然后 `cd` 到你想统计字数的那本书的目录，并更新它的 `book.toml` 文件。

```diff
  [book]
  title = "mdBook Documentation"
  description = "Create book from markdown files. Like Gitbook but implemented in Rust"
  authors = ["Mathieu David", "Michael-F-Bryan"]

+ [output.html]

+ [output.wordcount]
```

将书籍加载进内存时，`mdbook` 会检查你的 `book.toml` 文件，通过查找所有 `output.*` 表来确定要使用哪些后端。如果没有提供任何表，则会退回到默认的 HTML 渲染器。

值得注意的是，这意味着如果你想添加自己的自定义后端，还需要确保同时添加 HTML 后端，即使它的表保持为空。

现在你只需要像往常一样构建书籍，一切就应该能*直接正常工作*。

```shell
$ mdbook build
...
2018-01-16 07:31:15 [INFO] (mdbook::renderer): Invoking the "mdbook-wordcount" renderer
mdBook: 126
Command Line Tool: 224
init: 283
build: 145
watch: 146
serve: 292
test: 139
Format: 30
SUMMARY.md: 259
Configuration: 784
Theme: 304
index.hbs: 447
Syntax highlighting: 314
MathJax Support: 153
Rust code specific features: 148
For Developers: 788
Alternative Backends: 710
Contributors: 85
```

我们之所以不需要指定 `wordcount` 后端的完整名称/路径，是因为 `mdbook` 会按照约定尝试*推断*程序名。`foo` 后端的可执行文件通常命名为 `mdbook-foo`，并在 `book.toml` 中有对应的 `[output.foo]` 条目。如果想显式告诉 `mdbook` 要调用的命令（它可能需要命令行参数，或者是一个解释执行的脚本），可以使用 `command` 字段。

```diff
  [book]
  title = "mdBook Documentation"
  description = "Create book from markdown files. Like Gitbook but implemented in Rust"
  authors = ["Mathieu David", "Michael-F-Bryan"]

  [output.html]

  [output.wordcount]
+ command = "python /path/to/wordcount.py"
```

## 配置

现在假设你不想统计某一章的字数（该章可能是自动生成的文本/代码等）。实现这一点的标准方式是通过常用的 `book.toml` 配置文件，向你的 `[output.foo]` 表中添加相应的条目。

`Config` 大致可以看作一个嵌套的哈希表（hashmap），你可以调用 `get()` 之类的方法来访问配置的内容；此外还有一个便捷方法 `get_deserialized()`，可以取回某个值并自动将其反序列化为任意类型 `T`。

为了实现这一点，我们将创建自己的可序列化 `WordcountConfig` 结构体，用它来封装这个后端的全部配置。

首先，在你的 `Cargo.toml` 中添加 `serde` 和 `serde_derive`，

```
$ cargo add serde serde_derive
```

然后就可以创建配置结构体了，

```rust
use serde_derive::{Serialize, Deserialize};

...

#[derive(Debug, Default, Serialize, Deserialize)]
#[serde(default, rename_all = "kebab-case")]
pub struct WordcountConfig {
  pub ignores: Vec<String>,
}
```

现在我们只需要从 `RenderContext` 中反序列化出 `WordcountConfig`，然后添加一个检查，确保跳过被忽略的章节。

```diff
  fn main() {
      let mut stdin = io::stdin();
      let ctx = RenderContext::from_json(&mut stdin).unwrap();
+     let cfg: WordcountConfig = ctx.config
+         .get_deserialized("output.wordcount")
+         .unwrap_or_default();

      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
+             if cfg.ignores.contains(&ch.name) {
+                 continue;
+             }
+
              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
          }
      }
  }
```

## 输出与报告失败

在构建书籍时把字数统计打印到终端固然不错，但把它们输出到某个文件中可能也是个好主意。`mdbook` 通过 [`RenderContext`] 中的 `destination` 字段告诉后端应将生成的输出放在哪里。

```diff
+ use std::fs::{self, File};
+ use std::io::{self, Write};
- use std::io;
  use mdbook::renderer::RenderContext;
  use mdbook::book::{BookItem, Chapter};

  fn main() {
    ...

+     let _ = fs::create_dir_all(&ctx.destination);
+     let mut f = File::create(ctx.destination.join("wordcounts.txt")).unwrap();
+
      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
              ...

              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
+             writeln!(f, "{}: {}", ch.name, num_words).unwrap();
          }
      }
  }
```

> **注意：** 无法保证目标目录一定存在或为空（`mdbook` 可能会保留之前的内容，以便后端做缓存），所以最好始终用 `fs::create_dir_all()` 来创建它。
>
> 如果目标目录已经存在，不要假定它是空的。为了让后端能够缓存之前运行的结果，`mdbook` 可能会把旧内容留在目录中。

处理书籍的过程中总是可能出现错误（看看我们已经写了多少个 `unwrap()` 就知道了），因此 `mdbook` 会将非零退出码视为渲染失败。

例如，如果我们想确保所有章节的单词数都是*偶数*，遇到奇数就报错退出，那么可以这样写：

```diff
+ use std::process;
  ...

  fn main() {
      ...

      for item in ctx.book.iter() {
          if let BookItem::Chapter(ref ch) = *item {
              ...

              let num_words = count_words(ch);
              println!("{}: {}", ch.name, num_words);
              writeln!(f, "{}: {}", ch.name, num_words).unwrap();

+             if cfg.deny_odds && num_words % 2 == 1 {
+               eprintln!("{} has an odd number of words!", ch.name);
+               process::exit(1);
+             }
          }
      }
  }

  #[derive(Debug, Default, Serialize, Deserialize)]
  #[serde(default, rename_all = "kebab-case")]
  pub struct WordcountConfig {
      pub ignores: Vec<String>,
+     pub deny_odds: bool,
  }
```

现在，如果我们重新安装后端并构建一本书，

```shell
$ cargo install --path . --force
$ mdbook build /path/to/book
...
2018-01-16 21:21:39 [INFO] (mdbook::renderer): Invoking the "wordcount" renderer
mdBook: 126
Command Line Tool: 224
init: 283
init has an odd number of words!
2018-01-16 21:21:39 [ERROR] (mdbook::renderer): Renderer exited with non-zero return code.
2018-01-16 21:21:39 [ERROR] (mdbook::utils): Error: Rendering failed
2018-01-16 21:21:39 [ERROR] (mdbook::utils):    Caused By: The "mdbook-wordcount" renderer failed
```

你可能已经注意到，插件子进程的输出会立即透传给用户。我们鼓励插件遵循“沉默原则”（rule of silence），只在必要时才产生输出（例如生成过程中出错或发出警告时）。

所有环境变量都会透传给后端，因此你可以照常使用 `MDBOOK_LOG` 来控制日志的详细程度。

## 总结

虽然这个例子有些刻意，但希望它足以展示如何为 `mdbook` 创建一个替代后端。如果你觉得它还缺少什么，欢迎随时到 [issue tracker] 中提一个 issue，以便我们改进这份用户指南。

本章开头提到的那些现有后端，可以作为真实实践中的绝佳范例，欢迎随时浏览它们的源代码或提问。


[Third Party Plugins]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins
[`RenderContext`]: https://docs.rs/mdbook-renderer/latest/mdbook_renderer/struct.RenderContext.html
[`RenderContext::from_json()`]: https://docs.rs/mdbook-renderer/latest/mdbook_renderer/struct.RenderContext.html#method.from_json
[`semver`]: https://crates.io/crates/semver
[`Book`]: https://docs.rs/mdbook-renderer/latest/mdbook_renderer/book/struct.Book.html
[`Book::iter()`]: https://docs.rs/mdbook-renderer/latest/mdbook_renderer/book/struct.Book.html#method.iter
[`Config`]: https://docs.rs/mdbook-renderer/latest/mdbook_renderer/config/struct.Config.html
[issue tracker]: https://github.com/rust-lang/mdBook/issues
