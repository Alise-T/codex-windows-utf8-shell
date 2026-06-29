# windows-utf8-shell

一个给 Codex 使用的轻量 skill，用来降低 Windows PowerShell 环境下的中文和 UTF-8 编码问题。

## 为什么需要它

Codex 在 Windows 上执行命令时，经常会通过 PowerShell 读取文件、拼接脚本、运行 Python/Node、调用模型 API 或生成报告。

这些默认路径在处理中文时并不总是安全：

- `Get-Content` 默认读取方式可能不是 UTF-8
- PowerShell here-string、`echo`、管道传递内联脚本时，中文 prompt 可能在进入 Python 或 API 前已经损坏
- PowerShell 5.1 和 PowerShell 7 对 `-Encoding utf8` 的行为不完全一致
- 通过重定向、`Out-File`、未指定编码的写入方式生成中文文件时，可能出现 BOM、乱码或问号
- JSON 请求内容如果在 shell 层被破坏，模型收到的就不是原始中文

这类问题很容易被误判为“模型没理解中文”“API 返回异常”或“文件内容本来就坏了”。  
这个 skill 的目标是让 Codex 在触碰中文、UTF-8 文件、JSON 和 API 请求内容时，优先使用更稳的读取、写入和传输方式。

## 它主要解决的问题

- 读取或写入中文 Markdown、YAML、JSON、JSONL 时出现乱码
- PowerShell here-string、`echo`、重定向导致中文 prompt 被破坏
- 调用模型 API 时，中文请求内容变成 `??`、乱码或空回复
- Windows PowerShell 5.1 和 PowerShell 7 对 `-Encoding utf8` 行为不一致
- Codex 在处理中文文件、报告、日志、API 请求内容时误判为模型问题
