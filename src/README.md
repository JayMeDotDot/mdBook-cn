# 简介

<style>
    .mdbook-version {
        position: absolute;
        right: 20px;
        top: 60px;
        background-color: var(--theme-popup-bg);
        border-radius: 8px;
        padding: 2px 5px 2px 5px;
        border: 1px solid var(--theme-popup-border);
        font-size: 0.9em;
    }
</style>

<div class="mdbook-version">
Version: {{ mdbook-version }}
</div>

**mdBook** 是一个使用 Markdown 创建书籍的命令行工具。它非常适合用来编写产品或 API 文档、教程、课程材料，或任何需要整洁、易于导航且可自定义呈现的内容。

* 轻量的 [Markdown] 语法，让你更专注于内容本身
* 内置[搜索][search]支持
* 为多种不同语言的代码块提供彩色[语法高亮][syntax highlighting]
* [主题][Theme]文件允许自定义输出的格式
* [预处理器][Preprocessors]可以为自定义语法提供扩展并修改内容
* [后端][Backends]可以将输出渲染为多种格式
* 使用 [Rust][Rust] 编写，兼顾速度、安全与简洁
* 自动测试 [Rust 代码示例](cli/test.md)

本指南本身就是 mdBook 产物的一个示例。Rust 编程语言项目也在使用 mdBook，《[Rust 程序设计语言][trpl]》一书是 mdBook 实际应用的另一个很好的例子。

[Markdown]: format/markdown.md
[search]: guide/reading.md#搜索
[syntax highlighting]: format/theme/syntax-highlighting.md
[theme]: format/theme/index.html
[preprocessors]: format/configuration/preprocessors.md
[backends]: format/configuration/renderers.md
[Rust]: https://www.rust-lang.org/
[trpl]: https://doc.rust-lang.org/book/
[Rust code samples]: cli/test.md

## 贡献

mdBook 是免费的开源软件。你可以在 [GitHub][mdRepo] 上找到源代码，问题和功能需求可以提交到 [GitHub issue tracker]。

mdBook 依靠社区来修复缺陷和添加功能：如果你想做出贡献，请阅读 [CONTRIBUTING][dev] 指南，并考虑提交一个 [pull request]。

## 许可协议

mdBook-cn 的源代码和文档基于 [Mozilla Public License v2.0][MPL 2.0] 发布。

[mdRepo]: https://github.com/rust-lang/mdBook
[GitHub issue tracker]: https://github.com/rust-lang/mdBook/issues
[dev]: https://github.com/rust-lang/mdBook/blob/main/CONTRIBUTING.md
[pull request]: https://github.com/rust-lang/mdBook/pulls
[MPL 2.0]: https://www.mozilla.org/MPL/2.0/
