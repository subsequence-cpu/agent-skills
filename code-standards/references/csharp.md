# C# 开发规则

先读取 `core-engineering.md`。以 `.editorconfig`、Roslyn analyzer、dotnet format、目标框架和仓库约定为准；没有项目规范时采用 Google C# 风格。

## 文件、组织与命名

- 文件和目录使用 `PascalCase`；尽量让文件名与主要类型一致，每个文件通常只放一个核心类型。
- class、record、method、enum、public field/property/event、namespace 使用 `PascalCase`。
- local 和 parameter 使用 `camelCase`；private/protected/internal field 使用 `_camelCase`；interface 以 `I` 开头。
- 缩写按普通单词处理，如 `MyRpc` 而不是 `MyRPC`；修饰符不改变命名规则。
- using 置于 namespace 之前；`System` 组优先，其余按字母排序，并服从项目的 global using 约定。
- 修饰符按项目或 Google 顺序排列；成员按嵌套类型、静态/常量字段、实例字段/属性、构造/终结、方法分组，再按可见性排序。
- 将同一接口的实现集中放置；partial type 仅用于生成代码或明确的框架边界。

## 格式

- 没有格式化器时使用两空格缩进、100 列限制和 K&R 大括号；Tab 不用于缩进。
- 每行最多一条语句、每条语句最多一个赋值；即使可省略也使用大括号。
- `else`、`catch`、`finally` 与前一个右大括号同一行。
- 逗号、控制关键字后使用空格；括号内部不加空格；二元运算符两侧加空格。
- 普通续行缩进四空格；初始化器、lambda 等带大括号结构按块格式化。
- 使用 dotnet format 或仓库格式化器作为空白、换行和 import 排序的事实来源。

## 类型、成员与语言特性

- 类型明显或显式类型过长时使用 `var`；若隐藏基础数值类型、精度、可空性或领域含义，则写明类型。
- 使用语言关键字 `string`、`int` 等表达内置类型，除非访问静态成员需要 CLR 类型名。
- nullable reference type 已启用时保持准确注解，不用无根据的 `!` 消除警告。
- 优先不可变数据、只读字段和 init-only 属性；公开可变集合前明确所有权。
- 简单数据载体可使用 record，但必须审查值相等、序列化和版本兼容语义。
- 属性保持轻量且无意外 I/O；复杂或可能失败的行为使用方法。
- 不提供空 finalizer；非托管资源遵循标准 dispose pattern，普通资源使用 `using` 声明或语句。
- 避免反射、dynamic、unsafe 和自定义运算符；确需使用时限制边界并增加测试。

## 异步、异常与集合

- 异步调用链保持异步，不使用 `.Result`、`.Wait()` 或其他同步阻塞；库代码按项目策略处理同步上下文。
- 可取消操作接收并传播 `CancellationToken`；取消不转换成普通失败。
- 返回 `Task`/`Task<T>`；仅事件处理器使用 `async void`。
- 捕获最具体异常；保留原始堆栈，重新抛出使用 `throw;`；不使用异常完成正常分支。
- 参数验证使用准确异常类型和参数名；公共错误消息不得泄露内部路径或敏感数据。
- 暴露集合时选择准确接口和可变性；枚举延迟序列时避免多次执行和被释放资源。
- LINQ 链保持可读，避免在热点路径制造隐藏分配或多次枚举。

## 测试与验证

- 测试命名表达被测行为和条件；Arrange/Act/Assert 清晰但不机械注释。
- 时间、随机、文件系统、网络和环境依赖应可注入或隔离。
- 启用 nullable、analyzer 和 warnings-as-errors 等仓库设置；不得通过大范围 suppression 掩盖问题。
- 运行受影响项目的 format、build、test 和静态分析；检查异步死锁、资源释放、序列化和 API 兼容。

来源：<https://google.github.io/styleguide/csharp-style.html>
