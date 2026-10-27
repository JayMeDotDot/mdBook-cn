# clean 命令

clean 命令用于删除生成的书籍以及任何其他构建产物。

```bash
mdbook clean
```

#### 指定目录

`clean` 命令可以接受一个目录作为参数，用作书籍的根目录，而不是当前工作目录。

```bash
mdbook clean path/to/book
```

#### `--dest-dir`

`--dest-dir`（`-d`）选项允许你覆盖书籍的输出目录，该目录会被此命令删除相对路径将相对于当前目录进行解析。如果未指定，将默认使用 `book.toml` 中 `build.build-dir` 键的值，或者 `./book`。

```bash
mdbook clean --dest-dir=path/to/book
```

`path/to/book` 可以是绝对路径，也可以是相对路径。
