#### `--watcher`

有多种不同的后端可用于检测文件何时发生了变化。

* `poll` (默认) --- 每秒扫描一次文件系统来检查文件是否被修改。
* `native` --- 使用操作系统原生的机制在文件变化时接收通知。
  这样可以减少持续的固定开销，但可能不如基于 `poll` 的监视器可靠。更多信息请参见这些 issue：[#383](https://github.com/rust-lang/mdBook/issues/383) [#1441](https://github.com/rust-lang/mdBook/issues/1441) [#1707](https://github.com/rust-lang/mdBook/issues/1707) [#2035](https://github.com/rust-lang/mdBook/issues/2035) [#2102](https://github.com/rust-lang/mdBook/issues/2102)
