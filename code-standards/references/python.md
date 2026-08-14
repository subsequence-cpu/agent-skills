# Python 开发规则

先读取 `core-engineering.md`。以项目 Python 版本、pyproject 配置、formatter、linter 和 type checker 为准；没有项目规范时参考 Google Python Style Guide。

## 工具、文件与 import

- 运行项目指定的 formatter 与 linter；Google 指南常用 pylint，许多项目使用 Black/Pyink、Ruff、mypy 或 pyright，不能混用冲突配置。
- 模块文件使用 `.py` 和小写 `snake_case`，不得使用连字符；可执行文件仅在需要直接执行时添加 shebang。
- import 只用于 package 和 module，不直接依赖动态路径、副作用或通配符导入。
- 每行一个 import；标准库、第三方、项目包分组，组内排序并使用项目规定的绝对/相对 import。
- 禁止依赖传递 import；类型专用 import 需要时放入 `TYPE_CHECKING`，同时避免运行时循环依赖。
- module import 不应执行网络、文件写入、线程启动或业务注册之外的意外行为。

## 格式

- 没有格式化器时使用四空格缩进、禁用 Tab、正文行宽 80；长 URL、import 和工具不可拆分内容按指南例外处理。
- 不在语句末尾写分号，不使用反斜杠续行；在括号、方括号和大括号中隐式换行。
- 仅在语法需要或提升可读性时使用括号；不要为 return 或简单条件增加无意义括号。
- 多行列表、调用和字面量按 formatter 规则使用尾随逗号，闭合符号与结构对齐。
- 顶层定义之间、方法之间和逻辑段落之间使用一致空行；不使用空格进行脆弱列对齐。
- 字符串引号服从 formatter/项目约定；复杂插值使用 f-string，不用字符串拼接构造日志。

## 命名与文档

- function、method、variable、module 使用 `snake_case`；class 使用 `CapWords`；constant 使用 `UPPER_SNAKE_CASE`。
- protected member 使用单个 `_`；仅在 Python 名称改写确有必要时使用双前导下划线。
- 避免单字符、歧义缩写、类型编码和覆盖 built-in 的名称；窄索引、数学和 throwaway 变量可例外。
- public module、class、function、method 使用 docstring；首行概述行为，再说明 Args、Returns、Raises 和不明显副作用。
- 注释解释原因、约束和算法，不逐句翻译代码；TODO 包含可追踪所有者或 issue。
- 错误消息应可执行、语法完整且不泄露敏感信息；logging 使用延迟格式参数。

## 语言特性与数据

- comprehension 和 generator expression 只在短且清晰时使用；多层循环、复杂过滤或副作用改为普通循环。
- lambda 限制为短单表达式；需要名称、注释、类型或复杂逻辑时定义函数。
- 默认参数不得使用可变对象；使用 `None` sentinel 或不可变默认值并在函数内初始化。
- property 只用于轻量、无惊讶副作用的属性访问；昂贵或可失败操作使用方法。
- 避免可变全局状态；模块常量不可变，缓存和 singleton 必须明确并发与测试重置策略。
- 使用 context manager 管理文件、socket、锁和事务；不依赖析构或垃圾回收释放资源。
- 真值测试用于空容器和 None 之外的自然条件；检查 None 使用 `is None`，不将 0/空值与缺失混淆。
- 使用默认 iterator/operator，避免无必要的索引循环和中间列表。

## 异常、资源与并发

- 捕获最具体异常，保持 try block 狭窄；禁止裸 `except`、空处理和 `except Exception` 包围大段业务。
- 使用 `raise NewError(...) from exc` 保留原因；重新抛出原异常使用裸 `raise`。
- 不用异常实现正常控制流，但遵循 Python 的 EAFP 惯例时仍需限制异常范围。
- 清理使用 `with` 或 `try/finally`；发生异常时事务和临时资源必须恢复一致状态。
- 线程同步不得依赖内置操作“碰巧原子”；共享状态使用 lock、queue 或隔离所有者。
- asyncio 任务必须被 await、保留并处理异常；明确取消、超时、TaskGroup 和事件循环所有权。
- CPU 密集、I/O 密集和多进程选择应依据运行模型，不在库内部暗中创建无限 worker。

## 类型注解

- 新公共 API 和复杂内部边界应提供准确注解；简单局部可依赖推断。
- 使用现代内置泛型和 `X | None` 前确认最低 Python 版本；否则遵循项目 typing 语法。
- 避免 `Any` 扩散；不可信数据从 `object`/`Unknown` 通过校验、Protocol、TypedDict 或数据模型收窄。
- type alias、TypeVar、Protocol 和 overload 应解决真实 API 问题，不为追求完美类型增加难维护复杂度。
- 不用宽泛 `# type: ignore`；指定错误码并说明第三方或类型系统限制。

## 程序入口、测试与验证

- 可执行模块将主逻辑放入 `main()`，并由 `if __name__ == "__main__":` 调用。
- 测试隔离时间、随机、环境变量、cwd、locale、网络和全局 cache；fixture 必须可靠清理。
- 覆盖异常、边界、None、编码、路径、时区、并发取消和序列化。
- 运行 formatter、linter、type checker、unit/integration tests；依赖变更检查 lockfile、最低版本和安全风险。

来源：<https://google.github.io/styleguide/pyguide.html>
