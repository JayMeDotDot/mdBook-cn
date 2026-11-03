# 安装

有多种方式可以安装 mdBook 命令行工具。请从下面选择一种最适合你需求的方法。

如果你是为了自动部署而安装 mdBook，请查看[持续集成][continuous integration]章节，了解更多安装示例。

[continuous integration]: ../continuous-integration.md

## 预编译二进制文件

你可以在 [GitHub Releases 页面][releases] 下载可执行二进制文件。下载适用于你平台（Windows、macOS 或 Linux）的二进制文件并解压。压缩包中包含一个 `mdbook` 可执行文件，你可以运行它来构建你的书籍。

为了方便运行，请将该二进制文件的路径添加到你的 `PATH` 中。

[releases]: https://github.com/rust-lang/mdBook/releases

## 使用 Rust 从源码构建

要从源码构建 `mdbook` 可执行文件，你首先需要安装 Rust 和 Cargo。请按照 [Rust 安装页面][Rust installation page] 上的说明操作。mdBook 目前至少需要 Rust 1.88 版本。

安装好 Rust 之后，可以使用以下命令来构建并安装 mdBook：

```sh
cargo install mdbook
```

这会自动从 [crates.io] 下载 mdBook，进行构建，然后将其安装到 Cargo 的全局二进制目录（默认为 `~/.cargo/bin/`）中。

每当你想更新到新版本时，都可以再次运行 `cargo install mdbook`。该命令会检查是否存在更新的版本，如果找到新版本就会重新安装 mdBook。

要卸载，请运行命令 `cargo uninstall mdbook`。

[Rust installation page]: https://www.rust-lang.org/tools/install
[crates.io]: https://crates.io/

### 安装最新的 main 分支版本

发布到 crates.io 的版本会稍稍落后于 GitHub 上托管的版本。如果你需要最新版本，你可以自行构建 mdBook 的 git 版本。Cargo 让这件事变得***超级简单***！

```sh
cargo install --git https://github.com/rust-lang/mdBook.git mdbook
```

同样，请确保将 Cargo 的 bin 目录添加到你的 `PATH` 中。

## 修改与贡献

如果你有兴趣对 mdBook 本身进行修改，请查看[贡献指南][Contributing Guide]了解更多信息。

[Contributing Guide]: https://github.com/rust-lang/mdBook/blob/main/CONTRIBUTING.md
