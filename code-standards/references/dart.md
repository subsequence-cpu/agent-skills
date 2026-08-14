# Dart 开发规则

先读取 `core-engineering.md`。以 `dart format`、`analysis_options.yaml`、Dart/Flutter SDK 约束和 Effective Dart 为准。

## 文件、格式与命名

- 所有代码运行 `dart format`，不手工维护与格式化器冲突的布局。
- library、package、directory、source file 使用 `lowercase_with_underscores`；类型、extension、typedef 使用 `UpperCamelCase`。
- member、function、parameter、local、top-level variable 使用 `lowerCamelCase`；私有名称以单个 `_` 开头。
- constant 与其他变量使用同样 lowerCamelCase，除非仓库有一致的旧约定。
- acronym 按普通单词处理；名称表达领域含义，不重复类型和作用域已说明的信息。
- import 按 SDK、package、relative 等项目/工具规则排序；避免导入内部 `src/` 路径和无关库。

## 文档与库结构

- 公共 library、type、member 使用 `///` doc comment；首句提供独立摘要并说明契约，不复述名称。
- 文档引用标识符使用方括号链接，代码使用 fenced code；公开参数、返回、异常和副作用应可理解。
- 实现注释使用完整句子解释原因；不保留失效代码和无所有者 TODO。
- package 公共 API 通过明确 export 暴露，内部实现放入 `lib/src`；避免意外导出依赖类型。
- 保持 library 循环依赖和大而全 barrel 文件受控。

## 类型、空安全与语言特性

- 使用 sound null safety；不要通过 `!` 绕过未证明的不变量。
- 可推断且清晰时省略局部类型；公共 API、复杂泛型和语义关键处写明类型。
- 默认使用 `final` 局部和字段；真正的编译期常量使用 `const`，并利用 const constructor 表达不可变对象。
- 使用 collection literal、spread、if/for element 和字符串插值，避免冗长构造和拼接。
- 不检查 `== true`/`== false`；boolean 条件直接表达，nullable boolean 显式处理 null。
- 避免 `dynamic`；不确定输入使用 `Object?` 并通过 pattern/type check 收窄。
- class、mixin、extension 和 extension type 仅在其抽象语义明确时使用，不隐藏全局状态或重 I/O。

## API 设计与错误

- API 保持一致、简洁且易发现；参数过多时使用 named parameter，并为 required/optional 提供明确语义。
- 避免仅转发的 getter/setter；属性用于低成本、无参数、概念上类似字段的操作。
- 构造函数建立有效对象；命名构造函数表达不同创建语义，factory 只在确需缓存、子类型或重定向时使用。
- 可恢复失败抛出语义准确的 Exception 或返回项目统一结果；Error 表示程序错误。
- 捕获具体异常并保留 stack trace；不空 catch，不无上下文重抛。
- Stream、subscription、controller、isolate 和 I/O 资源必须有明确关闭与取消生命周期。

## 异步与 Flutter

- Future 必须 await、return 或显式交给项目认可的 unawaited 机制；不得静默丢失异步错误。
- async API 传播错误和取消模型；重试设置上限、退避和幂等性。
- Stream 明确单订阅/广播、错误、完成和 backpressure 语义。
- Flutter `build` 保持声明式且无副作用；I/O、导航和状态变更放在明确生命周期或状态所有者中。
- widget 尽可能 const；避免在 build 中创建长期 controller、subscription 或 Future。
- 状态管理遵循项目架构，不为小改动引入第二套模式。

## 测试与验证

- 测试覆盖 null safety、异步失败、Stream 完成/取消、序列化和 widget 生命周期。
- 使用 fake clock、受控 scheduler 和 mock/fake 边界，避免真实网络和不稳定时间等待。
- 运行 `dart format`、`dart analyze`、`dart test`；Flutter 项目运行对应 widget/integration 测试。

来源：<https://dart.dev/effective-dart>
