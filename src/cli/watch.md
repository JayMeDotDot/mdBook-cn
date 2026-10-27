# watch 命令

当你希望每次文件变更时都重新渲染你的书籍，`watch` 命令会很有用。你可以在每次文件变更后反复执行 `mdbook build`，但只需运行一次 `mdbook watch`，它就会监视你的文件，并在你修改任何文件时自动触发构建；这还包括重新创建仍然被 `SUMMARY.md` 引用的已删除文件！

#### 指定目录

`watch` 命令可以接受一个目录作为参数，将其用作书籍的根目录，而不是当前工作目录。

```bash
mdbook watch path/to/book
```

#### `--open`

当你使用 `--open` (`-o`) 选项时，mdbook 会在你的默认网页浏览器中打开渲染好的书籍。

#### `--dest-dir`

`--dest-dir` (`-d`) 选项允许你更改书籍的输出目录。相对路径会相对于当前目录进行解析。如果未指定，则默认使用 `book.toml` 中 `build.build-dir` 键的值，或 `./book`。

{{#include arg-watcher.md}}

#### 指定排除模式

`watch` 命令不会为书籍根目录中 `.gitignore` 文件所列出的文件自动触发构建。`.gitignore` 文件可以包含 [gitignore 文档](https://git-scm.com/docs/gitignore)中描述的文件模式。这对于忽略某些编辑器创建的临时文件很有用。

_注意：只会使用书籍根目录中的 `.gitignore`。全局的 `$HOME/.gitignore` 或父目录中的 `.gitignore` 文件不会被使用。_
