# JavaScript 开发规则

先读取[公共工程规则](core-engineering.md)。Google JavaScript Style Guide 已停止更新并建议迁移 TypeScript；维护现有 JavaScript 时仍按本规则与仓库 ESLint/Prettier、运行时和模块系统执行，不因风格任务擅自迁移语言。浏览器任务另读[前端工程规则](frontend-engineering.md)，服务端任务另读[后端工程规则](backend-engineering.md)。

## 文件、模块与依赖

- 文件名全小写，可按项目使用连字符或下划线，扩展名为 `.js`；源码使用 UTF-8 和空格缩进。
- 新代码优先 ES module；文件按 license、file overview、import、实现等项目顺序组织。
- import/export 不因列宽任意拆分，路径包含项目要求的扩展名；不得重复 import 同一文件。
- 使用 named export，禁止新增 default export；只导出真实公共符号，不暴露可变模块内部状态。
- 避免循环依赖；side-effect import 必须必要且在不明显时说明。
- import alias 保持原符号含义，只为解决真实冲突使用；依赖由模块顶部统一声明。

## 格式与语法

- 运行 Prettier/项目 formatter；没有配置时使用两空格缩进、80 列和分号。
- 即使语法允许省略，大多数控制流也使用大括号；K&R 风格，`else`/`catch` 与右大括号同行。
- 每行一条语句；长表达式按高层语法边界换行，不靠空格纵向对齐。
- 字符串引号服从 formatter；复杂插值使用 template literal，不使用反斜杠字符串续行。
- object/array literal 使用尾随逗号和简写属性的方式由 formatter 决定；不得用逗号运算符压缩逻辑。
- 数字非十进制前缀使用小写规范形式；解析外部数值后检查 NaN、Infinity 和范围。

## 变量、命名与类型语义

- 默认 `const`，只有重新赋值时使用 `let`，禁止 `var`；每条声明只声明一个变量。
- class、constructor、typedef 使用 UpperCamelCase；function、method、parameter、variable 使用 lowerCamelCase。
- 真正常量可使用 CONSTANT_CASE；不要仅因变量是 `const` 就大写。
- 名称描述用途，不编码类型、可见性或容器信息；避免晦涩缩写和无上下文单字符。
- 使用严格相等 `===`/`!==`，除有意同时匹配 null/undefined 且项目允许的狭义情况外不使用宽松相等。
- 不依赖隐式类型 coercion 完成重要逻辑；转换使用 `String`、`Number`、`Boolean` 并验证结果。

## 函数、对象与类

- function 保持聚焦；嵌套 callback 优先 arrow function，但需要动态 `this`、prototype method 或具名堆栈时使用普通函数。
- 不修改 built-in prototype，不创建隐式 global，不使用 `with`。
- 禁止对动态文本使用 `eval` 或 `Function`；动态属性访问必须来自受控 key。
- class 仅用于真实对象抽象；不要用 class 充当 namespace，模块已提供作用域。
- field 在构造阶段建立稳定 shape；private 状态使用项目支持的 private field/closure，不靠命名假装安全边界。
- object 复制和 spread 是浅操作；处理嵌套 mutable data 时明确所有权，不误以为已深拷贝。
- 避免 getter/setter 中的昂贵 I/O 和意外副作用；可失败行为用方法。

## Promise、错误与资源

- Promise 必须 await、return 或显式交给受控后台机制，禁止 unhandled rejection。
- 使用 async/await 表达顺序逻辑；并行独立操作使用 `Promise.all` 等，并明确部分失败语义。
- catch 只处理能增加价值的错误；保留 cause 和上下文，不记录后又重复抛到上层记录。
- 抛出 `Error` 或合适子类，不抛字符串、数字或普通对象。
- 事件监听、timer、stream、AbortController 和订阅必须有明确清理与取消生命周期。

## 文档、运行时边界与测试

- 避免 prototype pollution：验证外部 object key，不把不可信对象直接 merge 到配置或原型对象。
- 对网络、DOM、文件、进程或数据库等宿主边界，按对应前端或后端工程规则处理超时、验证、编码和资源限制。
- JSDoc 用于公共 API、复杂类型和非显然契约；TypeScript 可表达的项目不建立第二套矛盾类型系统。
- 测试覆盖异步失败、取消、事件清理、模块状态、时区、浮点和序列化；端到端行为按所属工程规则验证。
- 运行 formatter、ESLint 和测试；构建、bundle、浏览器兼容或服务运行验证按所属工程规则执行。

来源：<https://google.github.io/styleguide/jsguide.html>
