# 在持续集成中运行 `mdbook`

有许多服务（例如 [GitHub Actions] 或 [GitLab CI/CD]）可用于自动测试和部署你的书。

以下提供了一些关于如何配置你的服务以运行 mdBook 的通用指南。具体的配置示例可以在 [自动化部署][Automated Deployment] wiki 页面中找到。

[GitHub Actions]: https://docs.github.com/en/actions
[GitLab CI/CD]: https://docs.gitlab.com/ee/ci/
[Automated Deployment]: https://github.com/rust-lang/mdBook/wiki/Automated-Deployment

## 安装 mdBook

安装 mdBook 有几种不同的策略。具体方法取决于你的需求和偏好。

### 预编译二进制文件

也许最简单的方法是使用 [GitHub Releases 页面][releases] 上提供的预编译二进制文件。一种简单的做法是使用流行的 `curl` 命令行工具来下载可执行文件：

```sh
mkdir bin
curl -sSL https://github.com/rust-lang/mdBook/releases/download/v{{ mdbook-version }}/mdbook-v{{ mdbook-version }}-x86_64-unknown-linux-gnu.tar.gz | tar -xz --directory=bin
bin/mdbook build
```

关于这种方式的一些注意事项：

* 这种方式相对较快，并且不一定需要处理缓存问题。
* 这种方式不需要安装 Rust。
* 指定具体的 URL 意味着你必须手动更新脚本才能获得新版本。如果你想锁定某个特定版本，这可能反而是一个优点。不过，有些用户更喜欢在新版本发布时自动获取新版本。
* 这种方式依赖于 GitHub CDN 的可用性。

[releases]: https://github.com/rust-lang/mdBook/releases

### 从源码构建

从源码构建需要先安装 Rust。有些服务已经预装了 Rust，如果你的服务没有预装，则需要添加一个安装 Rust 的步骤。

安装好 Rust 之后，就可以使用 `cargo install` 来构建并安装 mdBook。
我们建议使用 SemVer 版本说明符，这样你就能获得最新的**非破坏性**（non-breaking）版本的 mdBook。
例如：

```sh
cargo install mdbook --no-default-features --features search --vers "^{{ mdbook-version }}" --locked
```

其中包含了几个推荐的选项：

* `--no-default-features` —— 禁用一些特性，例如 `mdbook serve` 所使用的 HTTP 服务器，它在 CI 上可能并不需要。
  这会显著缩短构建时间。
* `--features search` —— 禁用默认特性意味着你需要手动启用你想要的特性，例如内置的 [搜索][search] 功能。
* `--vers "^{{ mdbook-version }}"` —— 这将安装 `{{ mdbook-semver }}` 版本系列中最新的版本。但是，其后的语义化版本不兼容的版本（例如 `{{ mdbook-semver-break }}`）不会被安装，因为它们可能会破坏你的构建。如果你之前安装的是较旧的版本，Cargo 会自动升级 mdBook。如果你想固定使用 mdBook 的某个特定版本，请将 `^` 替换为 `=`。
* `--locked` —— 这将使用 mdBook 发布时所使用的依赖版本。如果不加 `--locked`，则会使用所有依赖的最新版本，这可能包含自上次发布以来的一些修复，但也可能（很少见）导致构建问题。

由于构建 mdBook 可能会比较慢，你可能需要研究一下缓存选项。

[search]: guide/reading.md#搜索

## 运行测试

你可能希望在每次推送更改或创建 pull request 时使用 [`mdbook test`] 运行测试。这可以用来验证书中的 Rust 代码示例。

这需要先安装 Rust。有些服务已经预装了 Rust，如果你的服务没有预装，则需要添加一个安装 Rust 的步骤。

除了确保安装了合适版本的 Rust 之外，只需在书籍目录下运行 `mdbook test` 即可，没有其他更多需要做的事情了。

你可能还想考虑运行其他类型的测试，例如检查失效链接的 [mdbook-linkcheck2]。或者，如果你有自己的样式检查、拼写检查或其他测试，也可以在 CI 中运行它们。

[`mdbook test`]: cli/test.md
[mdbook-linkcheck2]: https://github.com/marxin/mdbook-linkcheck2#continuous-integration

## 部署

你可能希望自动部署你的书。有些人可能希望在每次推送更改时都进行部署，而另一些人则可能只希望在打上特定发布标签时才部署。

你还需要了解如何将更改推送到你的 Web 服务的具体细节。例如，[GitHub Pages] 只需要将输出提交到一个特定的 git 分支上。其他服务可能需要使用 SSH 之类的工具连接到远程服务器。

基本流程是：你需要运行 `mdbook build` 生成输出，然后将文件（位于 `book` 目录中）传输到正确的位置。

之后，你可能需要考虑是否需要使 Web 服务上的缓存失效。

各种不同服务的示例请参见 [自动化部署][Automated Deployment] wiki 页面。

[GitHub Pages]: https://docs.github.com/en/pages

### 404 处理

mdBook 会自动生成一个 404 页面，用于处理失效链接。默认输出是位于书籍根目录下名为 `404.html` 的文件。像 [GitHub Pages] 这样的一些服务会自动将该页面用于失效链接。对于其他服务，你可能需要考虑将 Web 服务器配置为使用该页面，因为它能为读者提供返回书籍的导航。

如果你的书没有部署在域名的根路径下，那么你应该设置 [`output.html.site-url`] 选项，以便 404 页面能正常工作。它需要知道书部署在哪里，才能正确加载静态文件（例如 CSS）。例如，本指南部署在 <https://rust-lang.github.io/mdBook/>，其 `site-url` 设置如下：

```toml
# book.toml
[output.html]
site-url = "/mdBook/"
```

你可以在书中创建一个名为 `src/404.md` 的文件来自定义 404 页面的外观。如果你想使用不同的文件名，可以将 [`output.html.input-404`] 设置为其他文件名。

[`output.html.site-url`]: format/configuration/renderers.md#html-渲染器选项
[`output.html.input-404`]: format/configuration/renderers.md#html-渲染器选项
