# index.hbs

`index.hbs` 是用于渲染书籍的 handlebars 模板。markdown 文件会被处理为 html，然后注入到该模板中。

如果你想修改书籍的布局或样式，很可能就需要对这个模板做一些改动。以下是你需要了解的内容。

## 数据

大量数据通过 "context"（上下文）暴露给 handlebars 模板。在 handlebars 模板中，你可以这样访问这些信息：

```handlebars
{{name_of_property}}
```

以下是暴露出来的属性列表：

- ***language*** 书籍的语言，形如 `en`，由 `book.toml` 指定（如未指定，默认为 `en`）。例如可用于 <code class="language-html">\<html lang="{{ language }}"></code>。
- ***title*** 当前页面所用的标题。它等同于 `{{ chapter_title }} - {{ book_title }}`；若未设置 `book_title`，则默认就是 `chapter_title`。
- ***book_title*** 书籍的标题，由 `book.toml` 指定。
- ***chapter_title*** 当前章节的标题，取自 `SUMMARY.md` 中的条目。
- ***path*** 相对于源目录的原始 markdown 文件路径。
- ***content*** 渲染后的 markdown 内容。
- ***path_to_root*** 一个仅由 `../` 组成的路径，指向从当前文件到书籍根目录的相对位置。由于原始目录结构得以保留，把相对链接加上 `path_to_root` 前缀会很有用。
- ***previous*** 和 ***next*** 这两个对象用于链接到上一章和下一章，包含对应章节的 `title` 和 `link` 属性。
- ***chapters*** 是一个形如下式的字典数组：
  ```json
  {"section": "1.2.1", "name": "name of this chapter", "path": "dir/markdown.md"}
  ```
  包含书籍的全部章节。例如可用于构建目录（侧边栏）。

## Handlebars 助手函数

除了可以访问上述属性之外，你还可以使用一些 handlebars 助手函数。

### toc

toc 助手函数的用法如下：

```handlebars
{{#toc}}{{/toc}}
```

并根据你书籍的结构输出类似如下的内容：

```html
<ul class="chapter">
    <li><a href="link/to/file.html">Some chapter</a></li>
    <li>
        <ul class="section">
            <li><a href="link/to/other_file.html">Some other Chapter</a></li>
        </ul>
    </li>
</ul>
```

如果你想制作其他结构的目录，可以使用包含全部数据的 chapters 属性。目前唯一的限制是必须用 JavaScript 来实现，而不能借助 handlebars 助手函数。

```html
<script>
var chapters = {{chapters}};
// Processing here
</script>
```

### resource

静态文件的路径。它会隐式包含 `path_to_root`，并且能自动适配文件名中带哈希值的重命名文件。

```handlebars
<link rel="stylesheet" href="{{ resource "css/chrome.css" }}">
```

### fa

mdBook 内置了一份 [Font Awesome Free](https://fontawesome.com) MIT 许可的 SVG 文件副本。它接受三个位置参数：

1. 类型："solid"、"regular" 和 "brands" 之一（目前不支持 light 和 duotone）
2. 图标：从[免费图标集](https://fontawesome.com/v6/search)中任选
3. ID（可选）：如果提供，包裹图标的 `<span>` 标签上会加上相应的 HTML ID 属性

例如，这段 handlebars 语法会渲染成这样的 HTML：

```handlebars
{{fa "solid" "print" "print-button"}}
```

```html
<span class=fa-svg id="print-button"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M448 192V77.25c0-8.49-3.37-16.62-9.37-22.63L393.37 9.37c-6-6-14.14-9.37-22.63-9.37H96C78.33 0 64 14.33 64 32v160c-35.35 0-64 28.65-64 64v112c0 8.84 7.16 16 16 16h48v96c0 17.67 14.33 32 32 32h320c17.67 0 32-14.33 32-32v-96h48c8.84 0 16-7.16 16-16V256c0-35.35-28.65-64-64-64zm-64 256H128v-96h256v96zm0-224H128V64h192v48c0 8.84 7.16 16 16 16h48v96zm48 72c-13.25 0-24-10.75-24-24 0-13.26 10.75-24 24-24s24 10.74 24 24c0 13.25-10.75 24-24 24z"/></svg></span>
```
