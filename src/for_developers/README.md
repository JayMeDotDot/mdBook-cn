# 面向开发者

虽然 `mdbook` 主要作为命令行工具使用，但你也可以直接导入底层库，并用它们来管理一本书。它还提供了一套相当灵活的插件机制，如果你需要对书籍做一些分析，或者以不同的格式渲染它，你可以借此创建自己的自定义工具和消费者（通常称为 *backend*，后端）。

*For Developers*（面向开发者）章节将向你展示 `mdbook` 的更高级用法。

开发者介入书籍构建过程的两种主要方式是：

- [预处理器](preprocessors.md)
- [其他后端](backends.md)

## 构建过程

渲染一个书籍项目的过程需要经过以下几个步骤。

1. 加载书籍
    - 解析 `book.toml`，如果它不存在，则回退到默认的 `Config`
    - 将书籍章节加载到内存中
    - 确定应该使用哪些预处理器/后端
2. 对每个后端：
   1. 运行所有预处理器。
   2. 调用后端来渲染处理后的结果。

## 将 `mdbook` 用作库

`mdbook` 二进制程序只是底层 mdBook crate 的一个封装，它将这些 crate 的功能以命令行程序的形式暴露出来。如果你想以编程方式驱动 mdBook，可以使用 [`mdbook-driver`] crate。它可以用来添加你自己的功能，或者调整构建过程。

了解如何使用 `mdbook-driver` crate 的最简单方式是查阅 [API 文档][API Docs]。顶层文档解释了如何使用 [`MDBook`] 类型来加载和构建一本书，而 [config] 模块则对配置系统作了很好的说明。

[`MDBook`]: https://docs.rs/mdbook-driver/latest/mdbook_driver/struct.MDBook.html
[API Docs]: https://docs.rs/mdbook-driver/latest/mdbook_driver/
[config]: https://docs.rs/mdbook-driver/latest/mdbook_driver/config/index.html
