# mdBook 特有功能

## 隐藏代码行

mdBook 提供了一项功能，可以通过在代码行前添加特定前缀来隐藏这些行。

对于 Rust 语言，可以像 [Rustdoc 中那样][rustdoc-hide]，在行首加上 `# `（`#` 加一个空格）来隐藏该行。如果某行确实需要以字面字符串 `# ` 开头而不想被隐藏，可以用 `##` 来转义这个前缀（更多细节参见 [Rustdoc 文档][rustdoc-hide]）。

[rustdoc-hide]: https://doc.rust-lang.org/stable/rustdoc/write-documentation/documentation-tests.html#hiding-portions-of-the-example

```bash
# fn main() {
    let x = 5;
    let y = 6;

    println!("{}", x + y);
# }
```

会渲染为

```rust
# fn main() {
    let x = 5;
    let y = 6;

    println!("{}", x + y);
# }
```

当你点按或将鼠标悬停在代码块上时，会出现一个眼睛图标（<i class="fa fa-eye"></i>），用于切换隐藏行的可见性。

默认情况下，这只对标注为 `rust` 的代码示例生效。不过，你可以在 `book.toml` 中添加新的行隐藏前缀——指定语言名称和前缀字符——来为其他语言定义自定义前缀：

```toml
[output.html.code.hidelines]
python = "~"
```

该前缀会隐藏所有以给定前缀开头的行。以上面设置的 python 前缀为例，这段代码：

```bash
~hidden()
nothidden():
~    hidden()
    ~hidden()
    nothidden()
```

会渲染为

```python
~hidden()
nothidden():
~    hidden()
    ~hidden()
    nothidden()
```

可以在局部使用不同的前缀覆盖这一行为。下面的写法与上面的效果相同：

~~~markdown
```python,hidelines=!!!
!!!hidden()
nothidden():
!!!    hidden()
    !!!hidden()
    nothidden()
```
~~~

## Rust 演练场

Rust 语言的代码块会自动获得一个运行按钮（<i class="fas fa-play"></i>），点击即可执行代码，并把输出显示在代码块正下方。其原理是将代码发送到 [Rust 演练场][Rust Playground]。

```rust
println!("Hello, World!");
```

如果代码中没有 `main` 函数，代码会被自动包裹进一个 `main` 函数中。

如果想禁用某个代码块的运行按钮，可以像这样在代码块上加上 `noplayground` 选项：

~~~markdown
```rust,noplayground
let mut name = String::new();
std::io::stdin().read_line(&mut name).expect("failed to read line");
println!("Hello {}!", name);
```
~~~

或者，如果想禁用书中所有代码块的运行按钮，可以在 `book.toml` 中写入如下配置。

```toml
[output.html.playground]
runnable = false
```

## Rust 代码块属性

Rust 代码块可以在语言标识之后，附加以逗号、空格或制表符分隔的附加属性。例如：

~~~markdown
```rust,ignore
# This example won't be tested.
panic!("oops!");
```
~~~

在使用 [`mdbook test`] 测试 Rust 示例时，这些属性尤为重要。它们使用与 [rustdoc][rustdoc attributes]（rustdoc 属性）相同的属性，并额外增加了几项：

* `editable` --- 启用[编辑器][editor]。
* `noplayground` --- 移除运行按钮，但仍会被测试。
* `mdbook-runnable` --- 强制显示运行按钮。它适合与 `ignore` 属性结合使用，用于那些不希望被测试、但希望允许读者运行的示例。
* `ignore` --- 不会被测试，也不显示运行按钮，但仍会按 Rust 语法高亮。
* `should_panic` --- 执行时应当产生 panic。
* `no_run` --- 测试时会编译代码，但不运行。也不显示运行按钮。
* `compile_fail` --- 代码应当编译失败。
* `edition2015`、`edition2018`、`edition2021`、`edition2024` --- 强制使用特定的 Rust 版本（edition）。全局设置请参阅 [`rust.edition`]。

[`mdbook test`]: ../cli/test.md
[rustdoc attributes]: https://doc.rust-lang.org/rustdoc/documentation-tests.html#attributes
[editor]: theme/editor.md
[`rust.edition`]: configuration/general.md#rust-选项

## 引入文件

使用以下语法，可以把文件引入到你的书中：

```hbs
\{{#include file.rs}}
```

文件路径必须相对于当前源文件。

如果文件名包含空格，请用双引号包裹路径：

```hbs
\{{#include "file with spaces.rs"}}
```

闭引号之后的区间与锚点语法用法相同：

```hbs
\{{#include "file with spaces.rs":2:10}}
\{{#include "file with spaces.rs":my_anchor}}
```

在引号内，只有 `\"` 和 `\\` 会被当作转义序列；其他反斜杠会作为字面路径字符保留（这对Windows 风格的路径很有用）。未闭合的引号是一个错误。带引号的路径同样适用于 `rustdoc_include`。

mdBook 会将被引入的文件当作 Markdown 解析。由于 include 命令通常用于插入代码片段和示例，你常常需要用 ```` ``` ```` 将命令包裹起来，以便原样显示文件内容，而不会被当作 Markdown 解析渲染。

````hbs
```
\{{#include file.rs}}
```
````

## 引入文件的一部分

通常你只需要文件中的特定部分，例如示例相关的行。我们支持四种不同的部分引入模式：

```hbs
\{{#include file.rs:2}}
\{{#include file.rs::10}}
\{{#include file.rs:2:}}
\{{#include file.rs:2:10}}
```

第一条命令只引入文件 `file.rs` 的第二行。第二条命令引入直到第 10 行的所有行，即省略第 11 行直到文件末尾的内容。第三条命令引入从第 2 行开始的所有行，即省略第一行。最后一条命令引入 `file.rs`中第 2 到第 10 行组成的片段。

为了避免修改被引入的文件时破坏你的书，你也可以使用锚点而不是行号来引入特定章节。锚点是一对相互匹配的行。锚点的起始行必须匹配正则 `ANCHOR:\s*[\w_-]+`，同样，结束行必须匹配正则 `ANCHOR_END:\s*[\w_-]+`。这让你可以把锚点放在任何形式的注释行中。

例如下面这个待引入的文件：

```rs
/* ANCHOR: all */

// ANCHOR: component
struct Paddle {
    hello: f32,
}
// ANCHOR_END: component

////////// ANCHOR: system
impl System for MySystem { ... }
////////// ANCHOR_END: system

/* ANCHOR_END: all */
```

然后在书中，你只需这样写：

````hbs
Here is a component:
```rust,no_run,noplayground
\{{#include file.rs:component}}
```

Here is a system:
```rust,no_run,noplayground
\{{#include file.rs:system}}
```

This is the full file.
```rust,no_run,noplayground
\{{#include file.rs:all}}
```
````

被引入的锚点范围内，包含锚点模式的行会被忽略。

## 引入文件但初始时隐藏除指定行以外的内容

`rustdoc_include` 辅助命令用于从包含完整示例的外部 Rust 文件中引入代码，但初始时只显示指定的行——以与 `include` 相同的方式用行号或锚点来指定。

不在行号范围内或锚点之间的行仍会被包含进来，但它们会加上 `#` 前缀。这样，读者可以展开代码片段查看完整示例，而运行 `mdbook test` 时 Rustdoc 会使用完整的示例。

例如，假设有一个名为 `file.rs` 的文件，包含以下 Rust 程序：

```rust
fn main() {
    let x = add_one(2);
    assert_eq!(x, 3);
}

fn add_one(num: i32) -> i32 {
    num + 1
}
```

我们可以使用以下语法引入一个初始只显示第 2 行的片段：

````hbs
To call the `add_one` function, we pass it an `i32` and bind the returned value to `x`:

```rust
\{{#rustdoc_include file.rs:2}}
```
````

这与手动插入代码并用 `#` 隐藏除第 2 行以外的所有行的效果相同：

````hbs
To call the `add_one` function, we pass it an `i32` and bind the returned value to `x`:

```rust
# fn main() {
    let x = add_one(2);
#     assert_eq!(x, 3);
# }
#
# fn add_one(num: i32) -> i32 {
#     num + 1
# }
```
````

也就是说，它看起来像这样（点击“展开”图标可以查看文件的其余部分）：

```rust
# fn main() {
    let x = add_one(2);
#     assert_eq!(x, 3);
# }
#
# fn add_one(num: i32) -> i32 {
#     num + 1
# }
```

## 插入可运行的 Rust 文件

使用以下语法，可以把可运行的 Rust 文件插入到你的书中：

```hbs
\{{#playground file.rs}}
```

Rust 文件的路径必须相对于当前源文件。

点击运行时，代码片段会被发送到 [Rust 演练场][Rust Playground] 进行编译和运行。结果会被传回并直接显示在代码下方。

下面是渲染后的代码片段的样子：

{{#playground example.rs}}

文件名之后传递的任何附加值都会作为代码块的属性。例如 `\{{#playground example.rs editable}}` 会创建如下代码块：

~~~markdown
```rust,editable
# Contents of example.rs here.
```
~~~

`editable` 属性会启用[编辑器][editor]，如 [Rust 代码块属性](#rust-代码块属性)一节所述。

[Rust Playground]: https://play.rust-lang.org/

## 控制页面 \<title\>

章节可以在页面顶部附近加入 `\{{#title ...}}`，
来设置与目录（侧边栏）条目不同的 \<title\>。

```hbs
\{{#title My Title}}
```

## mdBook 提供的 HTML 类

<img class="right" src="images/rust-logo-blk.svg" alt="The Rust logo">

### `class="left"` 与 `"right"`

默认提供了这些类，用于通过行内 HTML 让图片浮动。

```html
<img class="right" src="images/rust-logo-blk.svg" alt="The Rust logo">
```

### `class="hidden"`

具有 `hidden` 类的 HTML 标签将不会显示。

```html
<div class="hidden">这个标签不会显示。/div>
```

<div class="hidden">这个标签不会显示。</div>

## Font-Awesome 图标

mdBook 内置了 [Font Awesome Free](https://fontawesome.com)第 6 版 MIT 许可 SVG 文件的副本。它模拟 `<i>` 语法，但会将结果转换为行内 SVG。只包含 regular、solid 和 brands 图标；light 图标等付费特性不包括在内。

例如，给定如下 HTML 语法：

```hbs
看起来像这样：<i class="fa-solid fa-print"></i>
```

看起来像这样：<i class="fa-solid fa-print"></i>

可用图标请参见[免费图标集](https://fontawesome.com/v6/search)。
