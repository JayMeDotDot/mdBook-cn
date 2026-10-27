# init 命令

每本新书都需要一些相同的、最基本的样板内容。为此，mdBook 提供了 `init` 命令。

`init` 命令的用法如下：

```bash
mdbook init
```

第一次使用 `init` 命令时，它会为你创建一些文件：

```bash
book-test/
├── book
└── src
    ├── chapter_1.md
    └── SUMMARY.md
```

- `src` 目录是你用 markdown 撰写书籍的地方。它包含所有的源文件、配置文件等。

- `book` 目录是渲染结果的存放位置。所有输出都可以直接上传到服务器，供你的读者查看。

- `SUMMARY.md` 是整本书的骨架，[另一章节](../format/summary.md)中有更详细的讨论。

#### 提示：根据 SUMMARY.md 生成章节

当 `SUMMARY.md` 文件已存在时，`init` 命令会先解析它，并根据其中使用的路径生成缺失的文件。这样，你可以先构思并创建整本书的结构，然后让 mdBook 为你生成对应的文件。

#### 指定目录

`init` 命令可以接受一个目录作为参数，将其用作书籍的根目录，而不是使用当前工作目录。

```bash
mdbook init path/to/book
```

#### `--theme`

使用 `--theme` 标志时，默认主题会被复制到你源目录下一个名为 `theme` 的目录中，以便你进行修改。

主题会被选择性地覆盖，也就是说，如果你不想覆盖某个特定的文件，只需将其删除，默认的文件就会被使用。

#### `--title`

为书籍指定标题。如果没有提供，交互式提示会要求输入一个标题。

```bash
mdbook init --title="my amazing book"
```

#### `--ignore`

创建一个 `.gitignore` 文件，用于忽略[构建]书籍时生成的 `book` 目录。如果没有提供，交互式提示会询问是否要创建该文件。

```bash
mdbook init --ignore=none
```

```bash
mdbook init --ignore=git
```

[构建]: build.md

#### `--force`

跳过创建 `.gitignore` 和输入书籍标题的交互式提示。
