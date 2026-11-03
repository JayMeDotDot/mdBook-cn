# 自定义字体

从 v0.5 起，字体被视为静态资源，因此会带有内容指纹（fingerprint）。[这曾引起困惑](https://github.com/rust-lang/mdBook/issues/2958)：该如何从 CSS 文件中引用它们。下面这个示例展示了具体做法：

提醒一下，存放在 `./theme/fonts/` 下的所有字体会被视为静态资源并带有内容指纹。另外，根据[主题文档](README.md#主题)，额外的字体可以在 `./theme/fonts/fonts.css` 中声明，该文件本身也被视为静态资源。

这让我们可以在 `/theme/fonts/fonts.css` 中使用 [`resource` 助手函数](index-hbs.html#resource)：

```css
@font-face {
    font-family: 'custom-font';
    src: url('{{ resource "fonts/Roboto.ttf" }}') format('truetype');
}
```

`{{ resource "fonts/Roboto.ttf" }}` 会被替换为该文件带内容指纹的文件名，这样一来，任何设置了 `font-family: custom-font` 的 HTML 块都会使用 Roboto 字体。

以我的情况为例：

```bash
$ ls book/fonts/
fonts-f9156add.css  Roboto-27bccdc7.ttf
```
