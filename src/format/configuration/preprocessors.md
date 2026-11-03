# 配置预处理器

预处理器是一种扩展，可以在原始 Markdown 源码发送给渲染器之前对其进行修改。

以下预处理器是内置的，并且默认启用：

- `links`：展开章节中的 `{{ #playground }}`、`{{ #include }}` 和 `{{ #rustdoc_include }}` handlebars
  辅助函数，以将文件的内容包含进来。
  详见 [引入文件][Including files]。
- `index`：将所有名为 `README.md` 的章节文件转换为 `index.md`。也就是说，
  所有 `README.md` 在渲染后的书籍中都会被渲染为索引文件 `index.html`。

可以通过 [`build.use-default-preprocessors`] 配置选项禁用这些内置预处理器。

社区已经开发了许多预处理器。可用预处理器的列表请参阅 [第三方插件][Third Party Plugins] wiki 页面。

有关如何创建新预处理器的信息，请参阅 [预处理器][Preprocessors for Developers] 一章。

[Including files]: ../mdbook.md#引入文件
[`build.use-default-preprocessors`]: general.md#构建选项
[Third Party Plugins]: https://github.com/rust-lang/mdBook/wiki/Third-party-plugins
[Preprocessors for Developers]: ../../for_developers/preprocessors.md

## 自定义预处理器配置

要添加预处理器，可以在 `book.toml` 中包含一个以预处理器名称命名的 `preprocessor` 表。例如，如果你有一个名为 `mdbook-example` 的预处理器，那么可以通过以下方式添加它：

```toml
[preprocessor.example]
```

有了这个表，mdBook 就会执行 `mdbook-example` 预处理器。

该表还可以包含该预处理器特有的额外键值对。例如，如果我们这个示例预处理器需要一些额外的配置选项：

```toml
[preprocessor.example]
some-extra-feature = true
```

## 将预处理器绑定到指定的渲染器

你可以通过将两者绑定在一起，显式指定某个预处理器应为哪个渲染器运行。

```toml
[preprocessor.example]
renderers = ["html"]  # example 预处理器仅在 HTML 渲染器下运行
```

## 提供你自己的命令

默认情况下，当你在 `book.toml` 文件中添加 `[preprocessor.foo]` 表时，`mdbook` 会尝试调用 `mdbook-foo` 可执行文件。如果你想使用不同的程序名或传入命令行参数，可以通过添加 `command` 字段来改变这一行为。

```toml
[preprocessor.random]
command = "python random.py"
```

### 可选的预处理器

如果你启用的预处理器尚未安装，默认行为是抛出错误。可以通过将预处理器标记为可选来改变这一行为：

```toml
[preprocessor.example]
optional = true
```

这会将错误降级为警告。

## 指定特定的执行顺序

预处理器的运行顺序可以通过 `before` 和 `after` 字段来控制。例如，假设你希望 `linenos` 预处理器处理那些可能通过 `{{#include}}` 包含进来的行，那么你会希望它在内置的 `links` 预处理器之后运行，这可以通过 `before` 或 `after` 字段来要求：

```toml
[preprocessor.linenos]
after = [ "links" ]
```

或者

```toml
[preprocessor.links]
before = [ "linenos" ]
```

在同一个配置文件中同时指定上面两者也是可行的，只不过是冗余的。

通过 `before` 和 `after` 指定了相同优先级的预处理器将按名称排序。任何无限循环都会被检测出来并产生一个错误。
