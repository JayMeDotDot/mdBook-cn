# SUMMARY.md 文件

摘要文件用于告知 mdBook 需要收录哪些章节、它们的先后顺序、层级结构,以及源文件的位置。没有这个文件,就没有书。

这个 markdown 文件必须命名为 `SUMMARY.md`。它的格式非常严格,必须遵循下文描述的结构,以便于解析。任何未在下文说明的元素,无论是格式上的还是文字上的,轻则被忽略,重则可能在构建书籍时引发错误。

### 结构

1. ***Title***(标题)- 虽然并非必需,但通常习惯以一个标题开头,一般为 <code class="language-markdown"># Summary</code>。不过解析器会忽略它,因此也可以省略。
   ```markdown
   # Summary
   ```

2. ***Prefix Chapter***(前缀章节)- 在主要编号章节之前,可以添加不参与编号的前缀章节。这对前言、引言等很有用。不过也有一些限制:前缀章节不能嵌套,必须全部位于根层级;而且一旦添加了编号章节,就不能再添加前缀章节。
   ```markdown
   [A Prefix Chapter](relative/path/to/markdown.md)

   - [First Chapter](relative/path/to/markdown2.md)
   ```

3. ***Part Title***(分部标题)- 一级标题可以作为其后编号章节的标题,用于在逻辑上划分书的不同部分。该标题会被渲染为不可点击的文本。分部标题是可选的,编号章节可以按需划分成任意多个部分。分部标题必须是一级标题(一个 `#`),其他级别的标题会被忽略。
   ```markdown
   # My Part Title

   - [First Chapter](relative/path/to/markdown.md)
   ```

4. ***Numbered Chapter***(编号章节)- 编号章节勾勒了全书的主体内容,并且可以嵌套,从而形成美观的层级结构(章节、子章节等)。
   ```markdown
   # Title of Part

   - [First Chapter](relative/path/to/markdown.md)
   - [Second Chapter](relative/path/to/markdown2.md)
      - [Sub Chapter](relative/path/to/markdown3.md)

   # Title of Another Part

   - [Another Chapter](relative/path/to/markdown4.md)
   ```
   
   编号章节可以用 `-` 或 `*` 来表示(不要混用两种符号)。

5. ***Suffix Chapter***(后缀章节)- 与前缀章节一样,后缀章节不参与编号,但位于编号章节之后。
   ```markdown
   - [Last Chapter](relative/path/to/markdown.md)

   [Title of Suffix Chapter](relative/path/to/markdown2.md)
   ```

6. ***Draft chapters***(草稿章节)- 草稿章节是没有对应文件、因而也没有内容的章节。设立草稿章节的目的,一是标记尚未撰写的未来章节;二是在规划书籍结构期间,避免在频繁调整结构时反复创建文件。在 HTML 渲染器中,草稿章节会在目录里渲染为禁用的链接,正如左侧目录中的下一章所示。草稿章节的写法与普通章节相同,只是不写文件路径。
   ```markdown
   - [Draft Chapter]()
   ```

7. ***Separators***(分隔线)- 分隔线可以添加在任何其他元素之前、之间或之后。它会在构建出的目录中渲染为一条 HTML 水平线。分隔线是一行只包含短横线且至少三个的内容:`---`。
   ```markdown
   # My Part Title
   
   [A Prefix Chapter](relative/path/to/markdown.md)

   ---

   - [First Chapter](relative/path/to/markdown2.md)
   ```
  
### 示例

下面是本指南 `SUMMARY.md` 的 markdown 源码,其渲染出的目录见左侧。

```markdown
{{#include ../SUMMARY.md}}
```
