# Swift 开发规则

先读取 `core-engineering.md`。遵循仓库 swift-format/SwiftLint、Swift 版本和部署目标；没有项目规范时使用 Google Swift Style，并兼顾 Apple API Design Guidelines。

## 文件与结构

- 文件以 `.swift` 结尾并使用 UTF-8；只用普通空格缩进，不使用 Tab。
- 主要包含单一类型时以类型命名文件；为协议一致性扩展时可使用 `Type+Protocol.swift`。
- 只 import 直接需要的顶层模块，不依赖传递 import；import 不换行并按普通模块、单个声明、`@testable` 分组排序。
- 文件通常只含一个主要顶层类型；紧密相关的小类型、delegate protocol 或 fileprivate helper 可同文件放置。
- 同名 overload 连续排列；extension 按职责或 protocol conformance 组织，不把类型任意切碎。
- 成员使用可解释的逻辑顺序，必要时用 `// MARK:` 标出分组，不按新增时间简单堆到文件末尾。

## 格式

- 没有格式化配置时采用 100 列限制和 K&R 大括号；不写分号，每行最多一条语句。
- 能在一行清晰表达且不超限时保持一行；逗号分隔列表要么全在一行，要么每项单独一行。
- 垂直列表和普通 continuation 按 Google 规则缩进；不要靠空格进行脆弱的横向对齐。
- 函数声明、调用、继承列表和 where clause 换行后保持结构边界清晰。
- 尾随闭包仅在主闭包语义清晰时使用；多闭包调用按当前 Swift 和仓库约定加标签。
- 数组、字典、参数和 case 的尾随逗号交给格式化器；同一文件保持一致。

## 命名与 API

- 类型、protocol 使用 UpperCamelCase；变量、函数、property、enum case 使用 lowerCamelCase。
- 调用点应自然表达语义；参数标签与函数名共同描述动作，避免重复类型信息。
- 缩写按单词处理并保持 Apple 生态约定；不要以 `_` 模拟 private，使用访问控制。
- boolean 名称读作断言，如 `isEmpty`、`hasValue`；factory 与 conversion 名称准确表达成本和失败可能。
- protocol 描述能力时使用名词或 `-able/-ible`；不在名称中加入无意义的 Protocol 后缀。
- 公共 API 记录泛型约束、actor isolation、错误和所有权语义，避免泄露实现类型。

## 类型、Optional 与错误

- 优先值语义和不可变 `let`；需要共享身份、继承或引用生命周期时使用 class。
- 使用 `if let`、`guard let`、optional chaining 或 nil-coalescing 处理 Optional。
- 避免 `!` 和 `as!`；只有局部、已证明且记录的不变量才允许强制操作。
- implicitly unwrapped optional 仅用于框架生命周期保证的边界，并尽快收窄。
- 使用 Swift 简写类型 `[Element]`、`[Key: Value]`、`T?`，除非泛型上下文要求显式形式。
- 可恢复失败使用 `throws` 或明确结果类型；enum error case 提供稳定、可匹配的语义。
- 不无理由使用 `try?` 丢失错误，也不在底层把所有错误转换为同一泛化错误。

## 控制流、属性与语言特性

- 使用 `guard` 处理前置条件和提前退出；不要为所有 if 机械改写 guard。
- switch 尽量穷尽，不使用无意义 default 掩盖新增 enum case；避免 `fallthrough`。
- computed property 应轻量、无意外副作用；可能失败、昂贵或执行 I/O 时使用方法。
- pattern matching 应提升清晰度；复杂 tuple pattern 和嵌套条件应拆分为具名逻辑。
- 不定义含义模糊的新运算符；重载现有运算符必须保持数学和标准库预期。
- 数值溢出操作符只在模运算确属领域语义时使用。

## 并发、生命周期与测试

- 使用结构化并发；不创建无所有者的 Task，必须明确取消、错误传播和完成等待。
- 尊重 actor isolation 与 `Sendable`；不得用 `@unchecked Sendable` 掩盖未审查共享状态。
- callback、Task 和 closure 捕获必须审查 retain cycle；weak 不是默认答案，应依据所有权选择。
- UI 更新在 MainActor；跨 actor 的数据应不可变或满足 Sendable。
- 测试覆盖 Optional、throw、取消、actor 切换、生命周期和协议一致性。
- 运行 swift-format、SwiftLint、编译警告和测试；保持部署目标与可用性标注一致。

来源：<https://google.github.io/swift/>
