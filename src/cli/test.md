# test 命令

编写书籍时，有时需要将一些测试自动化。例如，[Rust 程序设计语言](https://doc.rust-lang.org/stable/book/)（《Rust 程序设计语言》）使用大量可能过时的代码示例，因此对他们来说，能够自动测试这些代码示例非常重要。

mdBook 支持一个 `test` 命令，它会运行书中所有可用的测试。目前仅支持 Rust 测试。

#### 在代码块上禁用测试

rustdoc 不会测试包含 `ignore` 属性的代码块：

    ```rust,ignore
    fn main() {}
    ```

rustdoc 也不会测试指定了 Rust 以外语言的代码块：

    ```markdown
    **Foo**: _bar_
    ```

rustdoc *确实* 会测试未指定语言的代码块：

    ```
    This is going to cause an error!
    ```

#### 指定目录

`test` 命令可以接受一个目录作为参数，用它作为书籍的根目录，而不是当前工作目录。

```bash
mdbook test path/to/book
```

#### `--library-path`

`--library-path`（`-L`）选项允许你向 `rustdoc` 构建并测试示例时所使用的库搜索路径中添加目录。可以通过多个选项（`-L foo -L bar`）或以逗号分隔的列表（`-L foo,bar`）指定多个目录。该路径应指向 Cargo [构建缓存](https://doc.rust-lang.org/cargo/guide/build-cache.html)中包含项目构建输出的 `deps` 目录。例如，如果你的 Rust 项目的书籍位于名为 `my-book` 的目录中，以下命令在运行 `test` 时会包含该 crate 的依赖：

```shell
mdbook test my-book -L target/debug/deps/
```

更多信息请参阅 `rustdoc` 命令行[文档](https://doc.rust-lang.org/rustdoc/command-line-arguments.html#-l--library-path-where-to-look-for-dependencies)。

#### `--chapter`

`--chapter`（`-c`）选项允许你使用章节名称或章节的相对路径来测试书籍中的特定章节。
