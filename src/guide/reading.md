# 阅读书籍

本章将介绍如何与 mdBook 生成的书籍进行交互。这里假设你正在阅读一本 HTML 格式的书籍。对于其他输出格式（如 PDF），选项和排版会有所不同。

一本书由若干 *章节*（chapter）组成。每个章节是一个独立的页面。章节可以嵌套，形成子章节的层次结构。通常，每个章节会通过一系列 *标题*（heading）来进一步细分。

## 导航

有多种方法可以在本书的章节之间进行导航。

左侧的 **侧边栏** 列出了所有章节。点击任意章节标题即可加载该页面。

如果窗口太窄，侧边栏可能不会自动显示，在移动设备上尤其如此。在这种情况下，可以点击页面左上角的菜单图标（三条水平横线）来打开或关闭侧边栏。

页面底部的 **箭头按钮** 可以用于跳转到上一章或下一章。

键盘上的 **左右方向键** 也可以用于跳转到上一章或下一章。

## 顶部菜单栏

页面顶部的菜单栏提供了一些用于与书籍交互的图标。显示哪些图标取决于生成书籍时的配置。

| 图标 | 说明 |
|------|------|
| <i class="fas fa-bars"></i> | 打开或关闭章节列表侧边栏。 |
| <i class="fas fa-paintbrush"></i> | 打开选择器，用于切换不同的颜色主题。 |
| <i class="fas fa-magnifying-glass"></i> | 打开搜索栏，用于在书内搜索。 |
| <i class="fas fa-print"></i> | 指示浏览器打印整本书。 |
| <i class="fab fa-github"></i> | 打开指向本书源代码托管网站的链接。 |
| <i class="fas fa-pencil"></i> | 打开一个页面，直接编辑你当前正在阅读的页面的源文件。 |

点击菜单栏会使页面滚动到顶部。

## 搜索

每本书都内置了搜索系统。
点击菜单栏中的搜索图标 <i class="fas fa-magnifying-glass"></i>，或按下键盘上的 <kbd>/</kbd> 或 <kbd>S</kbd> 键，会打开一个用于输入搜索词的输入框。输入搜索词后，会实时显示匹配的章节和小节。

点击任意结果即可跳转到对应的小节。可以使用上下方向键在结果之间导航，按回车键则会打开当前高亮选中的小节。

加载搜索结果后，匹配的搜索词会在正文中高亮显示。点击高亮的词或按下 <kbd>Escape</kbd> 键即可取消高亮。

## 代码块

mdBook 常用于编程项目，因此支持对代码块和示例进行语法高亮。
代码块可能包含几个不同的图标，用于与之交互：

<style>
.light .table-wrapper .clip-button, .rust .table-wrapper .clip-button {
    --copy-button-filter: initial;
}
.coal .table-wrapper .clip-button {
    --copy-button-filter: brightness(0) saturate(100%) invert(72%) sepia(9%) saturate(401%) hue-rotate(167deg) brightness(90%) contrast(84%);
}
.navy .table-wrapper .clip-button {
    --copy-button-filter: brightness(0) saturate(100%) invert(88%) sepia(6%) saturate(563%) hue-rotate(200deg) brightness(89%) contrast(84%);
}
.ayu .table-wrapper .clip-button {
    --copy-button-filter: brightness(0) saturate(100%) invert(88%) sepia(2%) saturate(2%) hue-rotate(16deg) brightness(89%) contrast(94%);
}
</style>


| 图标 | 说明 |
|------|------|
| <i class="clip-button"></i> | 将代码块复制到本地剪贴板，以便粘贴到其他应用程序中。 |
| <i class="fas fa-play"></i> | 对于 Rust 代码示例，将会执行示例代码，并在示例下方显示编译器输出（参见 [playground]）。 |
| <i class="fa fa-eye"></i> | 对于 Rust 代码示例，将会切换“隐藏”行的可见性。有时，较大的示例会隐藏与所演示内容关系不大的行（参见 [hiding code lines]）。 |
| <i class="fas fa-clock-rotate-left"></i> | 对于 [可编辑的代码示例][editor]，将会撤销你所做的所有修改。 |

下面是一个示例：

```rust
println!("Hello, World!");
```

[editor]: ../format/theme/editor.md
[playground]: ../format/mdbook.md#rust-playground
[hiding code lines]: ../format/mdbook.md#hiding-code-lines
