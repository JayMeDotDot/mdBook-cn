# completions 命令

completions 命令用于为一些常见 shell 生成自动补全脚本。这意味着当你在 shell 中输入 `mdbook` 后，按下 shell 的自动补全键（通常是 Tab 键），它可能会显示有效的选项，或者补全你输入了一半的内容。

首先需要为你的 shell 安装补全脚本：

```bash
# bash
mdbook completions bash > ~/.local/share/bash-completion/completions/mdbook
# oh-my-zsh
mdbook completions zsh > ~/.oh-my-zsh/completions/_mdbook
autoload -U compinit && compinit
```

该命令会为指定的 shell 打印一个补全脚本。运行 `mdbook completions --help` 可以查看受支持的 shell 列表。

补全脚本放在哪里取决于你使用的 shell 以及你的操作系统。关于脚本应放在何处，请查阅你所使用 shell 的文档以了解更多信息。
