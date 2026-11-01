# 主题

默认渲染器使用 [handlebars](https://handlebarsjs.com) 模板来渲染你的 markdown 文件，并且 mdBook 二进制文件中自带一个默认主题。

主题是完全可定制的，你可以在项目根目录中 `src` 文件夹旁边添加一个 `theme` 目录，选择性地用你自己的文件替换主题中的每一个文件。只需创建一个与你想覆盖的文件同名的新文件，该文件就会取代默认文件被使用。

以下是你可以覆盖的文件：

- **_index.hbs_** 是 handlebars 模板。
- **_head.hbs_** 会被附加到 HTML 的 `<head>` 部分。
- **_header.hbs_** 的内容会附加在每一页书页的顶部。
- **_css/_** 包含用于设置书籍样式的 CSS 文件。
    - **_css/chrome.css_** 用于 UI 元素。
    - **_css/general.css_** 是基础样式。
    - **_css/print.css_** 是打印机输出的样式。
    - **_css/variables.css_** 包含其他 CSS 文件中使用的变量。
- **_book.js_** 主要用于添加客户端功能，比如隐藏 / 取消隐藏侧边栏、切换主题等。
- **_highlight.js_** 是用于高亮代码片段的 JavaScript，你通常不需要修改它。
- **_highlight.css_** 是代码高亮所使用的主题。
- **_favicon.svg_** 和 **_favicon.png_** 是将要使用的站点图标。SVG 版本会被[较新的浏览器][newer browsers]使用。
- **fonts/fonts.css** 包含要加载哪些字体的定义。自定义字体可以放在 `fonts` 目录中。

一般来说，当你想微调主题时，不需要覆盖所有文件。如果你只需要修改样式表，就没有必要覆盖其他所有文件。由于自定义文件的优先级高于内置文件，它们不会随新的修复 / 功能一起更新。

**注意：** 覆盖文件时，有可能会破坏某些功能。因此我建议使用默认主题中的文件作为模板，只添加 / 修改你需要的内容。你可以使用 `mdbook init --theme` 自动将默认主题复制到你的源码目录中，然后删除你不想覆盖的文件即可。

`mdbook init --theme` 并不会创建上面列出的所有文件。某些文件（例如 `head.hbs`）没有内置的对应版本。如果需要，直接创建该文件即可。

如果你完全替换了所有内置主题，请务必同时在配置中设置 [`output.html.preferred-dark-theme`]，它默认为内置的 `navy` 主题。

[`output.html.preferred-dark-theme`]: ../configuration/renderers.md#html-renderer-options
[newer browsers]: https://caniuse.com/#feat=link-icon-svg
