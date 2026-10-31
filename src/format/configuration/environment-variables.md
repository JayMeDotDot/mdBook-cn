# 环境变量

所有的配置值都可以通过设置相应的环境变量在命令行中进行覆盖。由于许多操作系统将环境变量限制为只能包含字母数字字符或 `_`，配置键的格式需要与正常的 `foo.bar.baz` 形式稍有不同。

以 `MDBOOK_` 开头的变量用于配置。配置键的生成方式是：去掉 `MDBOOK_` 前缀，然后把得到的字符串转换为 `kebab-case`。双下划线（`__`）用于分隔嵌套的键，而单个下划线（`_`）则会被替换为连字符（`-`）。

例如：

- `MDBOOK_book` -> `book`
- `MDBOOK_BOOK` -> `book`
- `MDBOOK_BOOK__TITLE` -> `book.title`
- `MDBOOK_BOOK__TEXT_DIRECTION` -> `book.text-direction`

因此，通过设置 `MDBOOK_BOOK__TITLE` 环境变量，你就可以覆盖书籍的标题，而无需改动你的 `book.toml`。

> **注意：** 为了便于设置更复杂的配置项，环境变量的值会首先被当作 JSON 来解析，如果解析失败则回退为字符串。
>
> 这意味着，如果你愿意，可以在构建书籍时用类似下面的方式覆盖所有的书籍元数据：
>
> ```shell
> $ export MDBOOK_BOOK='{"title": "My Awesome Book", "authors": ["Michael-F-Bryan"]}'
> $ mdbook build
> ```

在通过脚本或 CI 调用 `mdbook` 的场景中，有时无法在构建前更新 `book.toml`，这时后一种方式可能就很有用。
