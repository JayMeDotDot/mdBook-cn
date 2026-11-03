# 预处理器

*preprocessor*（预处理器）就是一小段代码，它在书籍加载之后、渲染之前立即运行，允许你更新和修改书籍。可能的用例包括：

- 创建类似 `\{{#include /path/to/file.md}}` 这样的自定义辅助指令
- 将 latex 风格的表达式（`$$ \frac{1}{3} $$`）替换为对应的 mathjax 形式

有关使用预处理器的更多信息，请参阅[配置预处理器](../format/configuration/preprocessors.md)。

## 接入 MDBook

MDBook 使用一种相当简单的机制来发现第三方插件。在 `book.toml` 中添加一个新的表（例如为 `foo` 预处理器添加 `[preprocessor.foo]`），然后 `mdbook` 就会在构建过程中尝试调用 `mdbook-foo` 程序。

预处理器定义完成、构建过程开始之后，mdBook 会执行 `preprocessor.foo.command` 键所定义的命令两次。第一次运行预处理器是为了判断它是否支持给定的渲染器。mdBook 会向该进程传递两个参数：第一个参数是字符串 `supports`，第二个参数是渲染器的名称。如果预处理器支持给定的渲染器，它应当以状态码 0 退出；如果不支持，则返回非零退出码。

如果预处理器支持该渲染器，mdbook 就会第二次运行它，并通过 stdin 传入 JSON 数据。该 JSON 由一个 `[context, book]` 数组组成，其中 `context` 是序列化后的 [`PreprocessorContext`] 对象，`book` 是一个包含书籍内容的 [`Book`] 对象。

预处理器应当将 [`Book`] 对象以 JSON 格式返回到 stdout，并附带上它想要执行的任何修改。

最简单的上手方式是自己实现 `Preprocessor` trait（例如在 `lib.rs` 中），然后再创建一个 shell 二进制程序，把输入转发到相应的 `Preprocessor` 方法。为方便起见，`examples/` 目录中提供了[一个空操作（no-op）预处理器的示例][an example no-op preprocessor]，它很容易就能改写成其他预处理器。

<details>
<summary>
Example no-op preprocessor
空操作预处理器示例
</summary>

```rust
// nop-preprocessors.rs

{{#include ../examples/nop-preprocessor.rs}}
```
</details>

## 实现预处理器的一些提示

通过将 `mdbook-preprocessor` 作为库引入，预处理器就可以使用现有的书籍处理基础设施。

例如，自定义预处理器可以使用 [`parse_input()`] 函数来反序列化写入 `stdin` 的 JSON。然后可以通过 [`Book::for_each_mut()`] 原地修改 `Book` 的每一章，最后再借助 `serde_json` crate 将结果写入 `stdout`。

章节既可以直接访问（递归地遍历各章节），也可以通过便捷方法 `Book::for_each_mut()` 访问。

`chapter.content` 只是一个恰好为 markdown 格式的字符串。虽然完全可以使用正则表达式或手动查找替换，但你可能更希望把输入处理成对计算机更友好的形式。[`mdbook-markdown`] crate 公开了 mdBook 用来解析 Markdown 的 [`pulldown-cmark`][pc] crate，而 [`pulldown-cmark-to-cmark`][pctc] crate 则可以把这些事件再转换回 markdown 文本。

下面的代码块展示了如何从 markdown 中移除所有强调（emphasis）标记，同时不会意外破坏文档。

```rust
{{#rustdoc_include ../examples/remove-emphasis/mdbook-remove-emphasis/src/main.rs:remove_emphasis}}
```

更多细节请查看[完整的示例源码][emphasis-example]。

## 用其他语言实现预处理器

mdBook 利用 stdin 和 stdout 与预处理器通信，这使得用 Rust 以外的语言实现预处理器变得很容易。下面的代码展示了如何用 Python 实现一个简单的预处理器，它会修改第一章的内容。下面的示例遵循上文展示的配置，其中 `preprocessor.foo.command` 实际指向一个 Python 脚本。

```python
import json
import sys


if __name__ == '__main__':
    if len(sys.argv) > 1: # 检查是否收到了任何参数
        if sys.argv[1] == "supports": 
            # 那么我们就可以直接返回退出状态码 0，因为另一个参数只是渲染器的名称
            sys.exit(0)

    # 从 stdin 加载 context 和 book 的表示形式
    context, book = json.load(sys.stdin)
    # 现在，我们只需修改第一章的内容
    book['items'][0]['Chapter']['content'] = '# Hello'
    # 书籍修改完成，直接把它打印到 stdout 即可
    print(json.dumps(book))
```


[emphasis-example]: https://github.com/rust-lang/mdBook/tree/main/examples/remove-emphasis/
[pc]: https://crates.io/crates/pulldown-cmark
[pctc]: https://crates.io/crates/pulldown-cmark-to-cmark
[an example no-op preprocessor]: https://github.com/rust-lang/mdBook/blob/main/examples/nop-preprocessor.rs
[`parse_input()`]: https://docs.rs/mdbook-preprocessor/latest/mdbook_preprocessor/fn.parse_input.html
[`Book::for_each_mut()`]: https://docs.rs/mdbook-preprocessor/latest/mdbook_preprocessor/book/struct.Book.html#method.for_each_mut
[`PreprocessorContext`]: https://docs.rs/mdbook-preprocessor/latest/mdbook_preprocessor/struct.PreprocessorContext.html
[`Book`]: https://docs.rs/mdbook-preprocessor/latest/mdbook_preprocessor/book/struct.Book.html
[`mdbook-markdown`]: https://docs.rs/mdbook-markdown/latest/mdbook_markdown/
