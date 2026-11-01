# 语法高亮

mdBook 使用 [Highlight.js](https://highlightjs.org) 并配合自定义主题进行语法高亮。

自动语言检测已被关闭，因此你很可能需要像这样指定所使用的编程语言：

~~~markdown
```rust
fn main() {
    // Some code
}
```
~~~

## 支持的语言

以下语言默认受支持，但你也可以通过提供自己的 `highlight.js` 文件来添加更多语言：

- apache
- armasm
- bash
- c
- coffeescript
- cpp
- csharp
- css
- d
- diff
- go
- handlebars
- haskell
- http
- ini
- java
- javascript
- json
- julia
- kotlin
- less
- lua
- makefile
- markdown
- nginx
- nim
- nix
- objectivec
- perl
- php
- plaintext
- properties
- python
- r
- ruby
- rust
- scala
- scss
- shell
- sql
- swift
- typescript
- x86asm
- xml
- yaml

## 自定义主题

与主题的其他部分一样，用于语法高亮的文件也可以用你自己的文件来覆盖。

- ***highlight.js*** 通常无需覆盖此文件，除非你想使用更新的版本。
- ***highlight.css*** highlight.js 用于语法高亮的主题。

如果你想为 `highlight.js` 使用其他主题，可以从其官网下载或自行制作，将其重命名为 `highlight.css` 并放到你书籍的 `theme` 文件夹中。

之后你的主题就会取代默认主题被使用。

## 改进默认主题

如果你认为默认主题在某种特定语言下的效果不太理想，或者还有改进空间，欢迎[提交新的 issue](https://github.com/rust-lang/mdBook/issues)说明你的想法，团队会进行查看。

你也可以创建 pull request 来提交所提议的改进。

总体而言，主题应当简洁素雅，不要使用过多花哨的颜色。
