# 编辑器

除了提供可运行的代码演练场（playground）之外，mdBook 还可以选择让它们可编辑。为了启用可编辑的代码块，需要在 ***book.toml*** 中添加以下内容：

```toml
[output.html.playground]
editable = true
```

启用可编辑代码块之后，还必须给代码块加上 `editable` 属性才能使其可编辑：

~~~markdown
```rust,editable
fn main() {
    let number = 5;
    print!("{}", number);
}
```
~~~

上面的代码会渲染出这个可编辑的演练场：

```rust,editable
fn main() {
    let number = 5;
    print!("{}", number);
}
```

注意可编辑演练场中新增的 `Undo Changes` 按钮。

## 自定义编辑器

默认情况下，编辑器是 [Ace](https://ace.c9.io/) 编辑器，但如果需要，可以通过提供一个不同的文件夹来覆盖其功能：

```toml
[output.html.playground]
editable = true
editor = "/path/to/editor"
```

注意，要让编辑器的更改正常工作，需要覆盖 `theme` 文件夹中的 `book.js`，因为它与默认的 Ace 编辑器存在一些耦合。
