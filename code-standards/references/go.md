# Go 开发规则

先读取 `core-engineering.md`。Go 规范以 `gofmt`/`goimports`、编译器、`go vet`、仓库 linter 和 Google Go Style Decisions/Best Practices 为准。

## 格式、包与文件

- 所有代码运行 `gofmt`；不要手工制定与格式化器冲突的空白规则。
- 使用 `goimports` 或项目工具维护 import；标准库、第三方和项目包按工具分组。
- 包名简短、小写、无下划线，不重复父目录或调用点已表达的含义。
- 文件按职责组织；平台和构建约束文件遵循 Go 命名与 build tag 规则。
- 避免 `dot import`；blank import 仅用于必要副作用，并在不明显时说明。
- 不通过 import cycle 规避设计问题；抽取更小、更稳定的依赖方向。

## 命名与 API

- 导出标识符使用大写开头，非导出使用小写；初始缩写保持一致，如 `URL`、`HTTP`、`ID`。
- 名称长度与作用域成正比；短循环索引可简短，跨函数和导出 API 应描述领域含义。
- getter 不使用 `Get` 前缀；setter 可用 `SetX`。接口常按行为命名，如 `Reader`、`Stringer`。
- 避免包名重复，如 `http.HTTPServer`；使调用点自然可读。
- 导出声明必须有以名称开头、说明契约的 doc comment。
- 公共 API 使用最小必要类型；不要为未来猜测提前暴露接口或配置字段。

## 类型、接口与数据

- 接口应小且由使用方定义；需要抽象时接收接口，默认返回具体类型。
- 不为模拟而把所有依赖变成接口；先确定真实替换边界。
- 使用零值有意义的类型；构造函数仅在必须建立不变量或依赖时提供。
- slice 与 map 的 nil/empty 语义必须与 API、JSON 和测试契约一致。
- 复制含 mutex、once 或其他不可复制状态的 struct 是错误；用指针传递并限制暴露。
- 选择值接收者或指针接收者后保持一致；需要修改、大对象、同步或身份语义时使用指针。
- 类型转换、整数宽度、时间 Duration、字节与字符串边界必须明确。

## 控制流与错误

- 先处理错误和边界并提前返回，减少 `else` 与深层嵌套。
- 每个返回错误都必须处理、传播或有意忽略；有意忽略时以代码或注释说明。
- 使用 `%w` 包装错误并增加操作上下文；调用方依赖分类时支持 `errors.Is`/`errors.As`。
- 错误字符串小写开头且通常不带句号，以便组合；不要重复记录后又向上传播。
- panic 只用于无法继续的程序不变量或初始化问题，不用于普通输入和可恢复失败。
- defer 用于可靠清理；循环中大量 defer 或捕获变量时检查生命周期和资源峰值。
- switch、type switch 和 range 应保持简单；注意 range 变量地址/闭包捕获与 Go 版本语义。

## 并发与 Context

- 启动 goroutine 前明确所有者、停止条件、错误通道和等待方式；不允许 goroutine 泄漏。
- `context.Context` 作为第一个参数传递，不存入 struct，不传 nil，不用于普通可选参数。
- 传播取消和 deadline；不要在底层无故替换调用方 Context。
- channel 由发送方或所有者关闭；接收方通常不关闭；不要用关闭 channel 发送额外值。
- 使用 mutex 保护不变量而不是单个字段；锁范围保持清晰，不在持锁时执行未知回调或慢 I/O。
- 并发任务应使用 `errgroup` 或项目抽象统一取消和错误收集；限制并发数量。

## 测试与工具

- 表驱动测试用于多个同构用例；子测试名称稳定且可读。
- 测试使用 `t.Helper()`、`t.Cleanup()` 和受控依赖；并行测试不得共享可变状态。
- 错误测试优先 `errors.Is`/`errors.As` 或稳定字段，不匹配完整错误文本。
- 运行 `gofmt`、`go vet`、`go test`，并按风险运行 race detector、fuzz、benchmark 和仓库 linter。
- 依赖变更运行 `go mod tidy` 时检查 diff，避免无关版本漂移。

来源：<https://google.github.io/styleguide/go/>
