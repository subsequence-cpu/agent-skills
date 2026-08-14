# Objective-C 开发规则

先读取 `core-engineering.md`。遵循 Google Objective-C Style、Apple Cocoa 命名与内存管理约定，以及仓库 clang-format、编译器和静态分析配置。

## 文件与格式

- 头文件使用 `.h`，实现使用 `.m`，Objective-C++ 仅在真实 C++ 边界使用 `.mm`。
- 头文件尽量自包含并最小化 import；可安全前置声明项目类型时使用 `@class`/`@protocol`，实现文件导入完整声明。
- 使用 UTF-8、两空格缩进和项目列宽；不使用 Tab，空白和换行交给格式化器。
- 方法声明和调用按冒号对齐或项目格式化规则换行；一个参数一行时保持关键字可读。
- 条件和循环始终使用大括号；K&R 风格放置大括号。
- import 分组并排序；主头文件优先，以暴露自包含问题。

## 命名与接口

- 类、category 和 protocol 使用项目唯一前缀，避免 Objective-C 全局运行时名称冲突。
- class/protocol 使用 UpperCamelCase；method、property、local variable 使用 lowerCamelCase；常量遵循项目统一前缀。
- selector 应使调用点读起来像自然语句，包含参数角色，不重复显而易见的类型。
- boolean property 使用肯定语义并适当采用 `is`/`has` getter；避免双重否定。
- init 方法以 `init` 开头并返回 `instancetype`；factory 方法返回 `instancetype`。
- 公共接口添加 nullability；集合使用轻量级泛型；避免把不准确的 `id` 扩散到业务代码。
- delegate property 通常为 weak，delegate method 第一个参数为发送者；可选行为按协议约定声明。

## 属性、对象与内存

- ARC 项目不调用 retain/release/autorelease/dealloc；非 ARC 边界服从所属模块策略。
- property 的 atomicity、readwrite/readonly、copy/strong/weak 必须与语义匹配；block 和 NSString 等可变来源通常审查是否需要 copy。
- 可变集合和对象不得无意暴露内部状态；返回不可变副本或受控接口。
- 在初始化器中建立完整不变量；失败返回 nil，并保持对象未发布。
- 避免在 `dealloc` 执行复杂逻辑；确保观察者、KVO、通知、timer 和 callback 生命周期成对清理。
- block 捕获 self 时审查循环引用和执行期限；weak/strong dance 只在确实需要时使用。
- Core Foundation 与 Objective-C 桥接必须明确所有权，使用正确 bridge 修饰。

## 方法、错误与并发

- 方法保持聚焦；参数过多时考虑值对象或 builder，但遵循现有 Cocoa API 风格。
- 普通可恢复失败使用 `NSError **`、result 或项目统一机制；异常只表示程序错误，不用于业务控制流。
- 检查 NSError domain/code/userInfo 稳定性，不把敏感内部信息暴露给用户。
- UI 对象只在主线程操作；后台任务必须明确队列、取消、回调线程和对象生命周期。
- dispatch queue 的串行/并行语义和 QoS 应与工作匹配；避免同步派发到当前队列造成死锁。
- 共享可变状态限制在串行队列、锁或其他明确同步所有者内。

## 现代语法、注释与测试

- 优先现代字面量、下标和泛型，但不为了简写改变 nil、异常或性能语义。
- 不使用已弃用 API；可用性检查和部署目标必须一致。
- 注释解释线程、所有权、可空性、不变量和框架限制；公共 API 使用 HeaderDoc/Doxygen 风格并与仓库一致。
- 测试覆盖初始化失败、nil、生命周期、通知/KVO、异步回调、主线程和 ObjC/Swift/C++ 互操作。
- 运行编译器警告、Clang Static Analyzer、格式化器和项目测试；新增警告不得通过宽泛 suppression 隐藏。

来源：<https://google.github.io/styleguide/objcguide.html>
