# TypeScript 开发规则

先读取 `core-engineering.md`。以 `tsconfig.json`、ESLint、Prettier、构建工具和 Google TypeScript Style Guide 为准；同时应用与类型不冲突的 JavaScript 运行时规则。

## 文件、模块与格式

- 使用项目 formatter；import/export、分号、引号和换行不得与自动工具冲突。
- 使用 ES module 和 named export，避免 default export；不得创建 import cycle 或意外 side-effect import。
- import type 仅用于类型依赖，并服从编译器的 verbatim/module 设置；不得通过 type-only import 隐藏真实运行时依赖。
- 文件围绕单一主题；不使用大而全 barrel 导出破坏 tree-shaking、初始化顺序和依赖可见性。
- template literal 用于复杂插值；禁止反斜杠字符串续行。
- 数值解析使用 `Number()` 或领域解析器，并检查 NaN、Infinity、空白输入和范围。

## 命名

- class、interface、type alias、enum 使用 UpperCamelCase；function、method、property、parameter、variable 使用 lowerCamelCase。
- 真正的模块常量使用 CONSTANT_CASE；不因 `const` 自动大写。
- 不使用 `I`、`T`、`Impl` 等前后缀机械编码类型或接口，除非仓库/框架已有明确约定。
- 不使用前后下划线模拟可见性；使用语言访问修饰符或 `#private`。
- 名称表达业务语义，不重复 namespace、类型和容器信息；单位应在类型或清晰名称中显式。
- generic type parameter 在简单局部可短，公共复杂 API 使用描述性名称。

## 类型建模

- 开启并保持项目 strict 选项；不通过降低全局 strictness 解决局部问题。
- 未知边界使用 `unknown`，验证后收窄；避免 `any`，不可避免时限制到最小适配层并说明。
- 使用小写 primitive 类型 `string`、`number`、`boolean`、`symbol`、`bigint`，不使用 boxed 类型。
- 优先准确的 union、literal、discriminated union 和 readonly shape，使非法状态不可表示。
- object shape 需要 declaration merging/extends 时使用 interface；其他组合可使用 type alias，项目一致性优先。
- 不把 `{}` 当作普通 object；根据语义使用 `object`、`unknown`、`Record` 或具体 interface。
- 数组、tuple、readonly collection 的可变性应准确；不要把 mutable collection 暴露为只读后又从别处修改。
- enum 使用必须审查运行时产物、序列化和真假转换；对外 wire value 优先稳定 string union 或明确 mapping。

## 收窄、断言与泛型

- 优先 `typeof`、`instanceof`、`in`、discriminant 和 type predicate 收窄，不用无验证的 `as`。
- 禁止双重断言 `as unknown as T` 掩盖不兼容；适配第三方时局部封装并运行时验证。
- 避免 non-null assertion `!`；若框架生命周期保证不变量，缩小范围并说明。
- generic 应表达输入输出关系；只出现一次、没有约束价值的 generic 改用具体/union 类型。
- overload 仅在调用结果确随输入签名变化时使用；实现签名不能成为对外 API。
- 条件类型、mapped type 和 template literal type 需要可理解、可诊断并有类型测试。

## 函数、类与运行时

- 默认 `const`，仅重新赋值用 `let`，禁止 `var`；函数参数和返回类型在公共边界显式。
- callback 类型写出参数和返回；不要用返回非 void 的函数满足 void callback 后忽略重要 Promise。
- async function 返回准确 Promise，所有 Promise 都必须处理；禁止 floating promise。
- class field 初始化顺序、decorator、metadata 和 emit 受 tsconfig 影响，修改前确认运行时语义。
- 倾向组合和纯函数；class 仅用于状态、身份、框架或多态的真实需求。
- getter 应低成本、无意外 I/O；可能失败或异步的行为使用方法。

## 边界、安全与测试

- TypeScript 类型在运行时不存在；HTTP、JSON、环境变量、storage 和第三方数据必须运行时验证。
- JSON 解析结果先视为 unknown；禁止仅靠 assertion 信任外部 shape。
- DOM、SQL、命令、URL 等输出执行上下文匹配的编码和 allowlist。
- library 公共类型避免泄露私有依赖实现；检查 declaration emit、API Extractor 或项目兼容工具。
- 测试运行时行为，也通过项目工具测试关键类型推断和 `@ts-expect-error`；禁止无原因 `@ts-ignore`。
- 运行 formatter、ESLint、`tsc --noEmit`、unit/integration tests 和构建；依赖升级检查类型版本、module resolution 和产物大小。

来源：<https://google.github.io/styleguide/tsguide.html>
