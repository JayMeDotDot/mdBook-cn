# MathJax 支持

mdBook 通过
[MathJax](https://www.mathjax.org/) 提供对数学公式的可选支持。

要启用 MathJax，需要在 `book.toml`
的 `output.html` 部分添加 `mathjax-support` 键。

```toml
[output.html]
mathjax-support = true
```

>**注意：** MathJax 常用的定界符目前尚不支持。现在还不能
使用 `$$ ... $$` 作为定界符，而 `\[ ... \]` 定界符需要
多加一个反斜杠才能生效。希望这一限制能尽快解除。

>**注意：** 在 MathJax 块中使用双反斜杠时（例如
> `\begin{cases} \frac 1 2 \\ \frac 3 4 \end{cases}` 这样的命令），需要
> 额外添加 _两个_ 反斜杠（例如 `\begin{cases} \frac 1 2 \\\\ \frac 3 4
> \end{cases}`）。

### 行内公式

行内公式由 `\\(` 和 `\\)` 定界。例如，要渲染下面这个行内公式
\\( \int x dx = \frac{x^2}{2} + C \\)，你需要
这样写：

```
\\( \int x dx = \frac{x^2}{2} + C \\)
```

### 块级公式

块级公式由 `\\[` 和 `\\]` 定界。要渲染下面的
公式

\\[ \mu = \frac{1}{N} \sum_{i=0} x_i \\]

你需要这样写：

```bash
\\[ \mu = \frac{1}{N} \sum_{i=0} x_i \\]
```
