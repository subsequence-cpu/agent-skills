# Kotlin 开发规则

先读取 `core-engineering.md`。遵循仓库 ktfmt/ktlint/detekt、Kotlin 版本和目标平台；Android 项目没有本地规范时采用 Google Android Kotlin Style。

## 文件与结构

- 源文件使用 UTF-8；单一顶层 class 时以大小写一致的类名加 `.kt` 命名。
- 多个顶层声明应围绕同一主题并使用描述性 PascalCase 文件名；无关公共声明拆分。
- 文件顺序为 license、file annotation、package、import、顶层声明，各存在部分之间空一行。
- package 和 import 不换行；所有 import 在同一列表按 ASCII 排序，禁止通配符 import。
- 源码自上而下可读，声明顺序应帮助理解；同一 class 内按逻辑分组，不机械按可见性或时间堆放。
- 重载连续放置；companion object、constructor、property 和 method 的位置服从可读性与仓库约定。

## 格式

- 没有格式化器时使用四空格缩进，不使用 Tab；continuation 至少额外四空格并保持结构清晰。
- 不写分号；一行一条语句；大括号采用 K&R 风格，`else`/`catch` 与右大括号同一行。
- 长参数、类型参数、supertype 和 chain 按语法单元换行；格式由 ktfmt/ktlint 统一。
- Lambda 为最后一个参数时可使用尾随 lambda；多个 lambda 或含义不清时保留具名参数。
- 尾随逗号是否使用由项目和格式化器决定，同一代码域保持一致。
- expression body 仅用于短且清晰的单表达式；复杂分支和调试需要时使用 block body。

## 命名与 API

- class、object、interface、enum 使用 UpperCamelCase；function、property、parameter、local 使用 lowerCamelCase。
- 常量仅对 compile-time constant 或深度不可变全局值使用 `UPPER_SNAKE_CASE`。
- backing property 只在必要时使用前导下划线；普通 private 不靠命名模拟访问控制。
- package 全小写且语义明确；避免 Java 式 `get`/`set` 前缀，优先 property。
- extension 应属于接收者或调用方的清晰领域，不用 extension 隐藏重依赖、I/O 或意外副作用。
- 公共 API 显式写出返回类型，保持 Java 互操作、二进制兼容、nullability 和序列化契约。

## 类型与语言特性

- 使用 Kotlin nullable type 和安全调用，避免 `!!`；不确定边界先验证或显式失败。
- 优先 `val` 和不可变集合；需要 mutation 时限制所有权与作用域。
- 使用 data class 表示真正的值对象，并审查自动 equals/hashCode/copy 是否符合身份和敏感字段语义。
- 用 sealed class/interface 建模有限状态，用 enum 表达无载荷固定值；switch 式 `when` 应穷尽。
- 避免滥用 operator、infix、reified 和委托；只有调用语义自然且收益明确时使用。
- collection chain 保持可读，热点路径检查中间集合；需要惰性时有意识地使用 Sequence。
- 与 Java 平台类型交互时尽快收窄 nullability，不把不安全类型扩散到内部。

## 协程、异常与资源

- 使用结构化协程和明确 scope；应用任务禁止 `GlobalScope`。
- dispatcher 在架构边界注入或选择，不在深层业务函数硬编码线程策略。
- 传播取消；不得捕获并吞掉 `CancellationException`，清理放入 `finally` 或 `use`。
- suspend API 明确超时、重试和幂等性；Flow 明确 cold/hot、buffer、backpressure 和生命周期。
- 可恢复错误遵循项目统一的 exception/result 模型；不以 null 模糊“没有值”和“失败”。
- Closeable 资源使用 `use`；Android lifecycle 中取消观察和任务。

## 测试与验证

- 测试覆盖 null、sealed 状态、协程取消/超时、Flow 顺序、Java 互操作和序列化。
- 使用测试 dispatcher/虚拟时间，避免真实 delay 和共享全局 scope。
- 运行 formatter、detekt、编译、单元/仪器测试；公开库检查 binary/API compatibility。

来源：<https://developer.android.com/kotlin/style-guide>
