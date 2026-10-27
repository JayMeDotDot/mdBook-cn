# serve 命令

serve 命令用于预览书籍，它默认通过 HTTP 在 `localhost:3000` 上提供服务：

```bash
mdbook serve
```

`serve` 命令会监视书籍的 `src` 目录的变动，每次变动后都会重新构建书籍并刷新客户端；这包括重新创建 `SUMMARY.md` 中仍然提到但已被删除的文件！客户端刷新是通过 websocket 连接触发的。

***注意：*** *`serve` 命令用于测试书籍的 HTML 输出，并不是要作为一个完整的网站 HTTP 服务器来使用。*

#### 指定目录

`serve` 命令可以接受一个目录作为参数，将其用作书籍的根目录，而不是使用当前工作目录。

```bash
mdbook serve path/to/book
```

### 服务器选项

serve 的主机名默认为 `localhost`，端口默认为 `3000`。这两个选项都可以在命令行上指定：

```bash
mdbook serve path/to/book -p 8000 -n 127.0.0.1 
```

#### `--open`

使用 `--open`（`-o`）标志时，mdbook 会在启动服务器后用你的默认网页浏览器打开这本书。

#### `--dest-dir`

`--dest-dir`（`-d`）选项允许你更改书籍的输出目录。相对路径将相对于当前目录进行解析。如果未指定，则默认使用 `book.toml` 中 `build.build-dir` 键的值，或者为 `./book`。

{{#include arg-watcher.md}}

#### 指定排除模式

对于书籍根目录下 `.gitignore` 文件中列出的文件，`serve` 命令不会自动触发构建。`.gitignore` 文件可以包含 [gitignore 文档](https://git-scm.com/docs/gitignore)中描述的文件模式。这对于忽略某些编辑器创建的临时文件很有用。

***注意：*** *只有书籍根目录下的 `.gitignore` 会被使用。全局的 `$HOME/.gitignore` 或父目录中的 `.gitignore` 文件不会被使用。*
