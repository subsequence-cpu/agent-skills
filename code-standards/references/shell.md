# Shell/Bash 开发规则

先读取 `core-engineering.md`。以仓库声明的 shell、ShellCheck、shfmt 和部署环境为准；Google 指南默认可执行脚本使用 Bash，但项目可能要求 POSIX sh。

## 使用边界与入口

- Shell 只用于小型工具或简单命令包装；出现复杂数据处理、非直线控制流、性能要求或约百行以上规模时改用结构化语言。
- 可执行脚本使用准确 shebang；Google Bash 环境使用 `#!/bin/bash`，不得为“兼容”擅自改变项目目标 shell。
- 库文件通常不需要可执行位和 shebang；文件扩展名与项目约定一致。
- 禁止 SUID/SGID shell script；需要权限边界时使用更安全的专用程序或受控工具。
- 输出正常结果到 stdout，诊断和错误到 stderr；机器可读输出必须保持稳定。

## 格式与注释

- 使用两空格缩进和不超过约 80 列的可读布局，长命令按语义断行；最终以 shfmt/项目工具为准。
- pipeline 过长时每段单独一行并让管道结构清晰；检查 pipeline 中各命令失败语义。
- `if`、`for`、`while`、`case` 使用标准换行和缩进；case pattern 与终止符保持一致。
- 文件头说明用途；非显然函数记录用途、参数、输出和返回码；实现注释解释陷阱而非复述命令。
- TODO 使用项目认可格式和可追踪责任信息。

## 展开、引用与命令

- 默认引用变量和命令替换：`"${value}"`、`"$(command)"`；只有有意分词或 glob 时才不引用并说明。
- 使用 `${var}` 明确边界；数组展开使用正确的 `"${array[@]}"`。
- 使用 `$(...)` 而不是反引号；Bash 条件使用 `[[ ... ]]`，算术使用 `(( ... ))`。
- 字符串测试使用 `-z`/`-n` 或明确比较；不要把未定义、空字符串和数值 0 混为一谈。
- 不使用 `eval` 拼接不可信内容；参数用数组构造，命令名必须来自受控 allowlist。
- 不解析 `ls`；使用 glob、`find -print0` 和 NUL-safe 读取处理文件名。
- 使用 shell builtin 而非无必要外部进程，但不能为微优化牺牲清晰度或可移植性。

## 变量、函数与作用域

- function 和普通 variable 使用小写 `snake_case`；constant 与 exported environment variable 使用 `UPPER_SNAKE_CASE`。
- Bash 函数内使用 `local`，声明与命令替换分开以免掩盖返回码。
- readonly 值在初始化后声明只读；不要覆盖 PATH、HOME 等系统变量表达业务状态。
- 函数放在调用前，并提供简短 `main` 组织顶层流程；脚本被 source 时避免自动执行 main。
- 参数通过 `"$@"` 传播；使用 `getopts` 或成熟解析器处理选项，不手写含糊解析。

## 错误、资源与安全

- 检查关键命令返回值；错误处理应显示上下文并返回非零，不只依赖最后一条命令。
- `set -euo pipefail` 不是通用修复，只在理解条件、subshell、pipeline 和 unset 语义后按项目采用。
- 临时文件/目录使用 `mktemp`，权限最小，并用 trap 可靠清理；不得使用可预测路径。
- 远程命令、curl、package manager 等设置超时、校验和/签名以及明确失败选项。
- 不在 `set -x`、进程参数或日志中暴露 secret；敏感输入通过受控 fd、stdin 或 secret store。
- 路径操作前验证目标，不把未解析变量、宽泛 glob 或用户输入直接交给删除和覆盖命令。

## 测试与验证

- 运行 ShellCheck 和 shfmt；只在明确误报且有理由时局部禁用规则。
- 测试空参数、含空格/换行/通配符文件名、失败命令、部分输出、信号和清理。
- 在目标 shell 与支持平台实际运行；不得用 Bash 测试结果声称 POSIX sh 兼容。

来源：<https://google.github.io/styleguide/shellguide.html>
