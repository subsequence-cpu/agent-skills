# Common Lisp 开发规则

先读取 `core-engineering.md`。以项目实现、ASDF、package、formatter 和 Google Common Lisp Style Guide 为准；不要把其他 Lisp 方言规则直接套用。

## 文件、包与系统

- 通过 ASDF 明确 system、component、依赖和加载顺序；源码文件围绕一个 package 或清晰模块职责组织。
- 每个文件使用明确 `in-package`，避免长期停留在 `CL-USER`；package export 只暴露稳定 API。
- `defpackage` 精确 import/use/export，避免名称冲突和无意 shadow；内部符号不通过双冒号从外部依赖。
- 编译和加载阶段副作用必须明确；不在加载普通库时执行网络、修改用户环境或启动后台任务。
- 遵循目标实现和标准版本，不依赖未声明实现扩展。

## 命名与格式

- 普通 symbol 使用描述性 `kebab-case`；special variable 使用 `*earmuffs*`，constant 使用 `+plus-signs+`。
- predicate 通常以 `-p` 结尾；破坏性操作遵循可辨识约定，不伪装成纯函数。
- class、condition、generic function 和 method 名称表达领域语义，不在名称中重复 package 信息。
- 使用 Lisp formatter/编辑器按 form 结构缩进；不手工用空格对齐形成易碎布局。
- 一行一条逻辑 form；长 lambda list、let binding 和 condition clause 按结构换行。

## 函数、宏与数据

- 优先函数；只有需要控制求值、创建语法或编译期转换时使用 macro。
- macro 应短小，避免重复求值、变量捕获和隐藏控制流；使用 gensym 并记录求值次数与顺序。
- lambda list 保持可理解；过多 optional/key 参数时考虑结构化配置，并检测未知关键字。
- 明确 destructive 与 non-destructive collection 操作，不在调用方仍持有共享结构时意外修改。
- 使用适当 sequence、hash table、structure 或 class，不以 property list 代替有不变量的数据模型。
- 比较函数按对象语义选择 `eq`、`eql`、`equal` 或 `equalp`，不得随意互换。
- special/global 状态限制作用域并动态绑定；普通依赖通过参数或对象传递。

## Condition、资源与并发

- 用 condition system 表达可恢复问题；在能够决定恢复策略的层使用 handler，在协议边界提供合理 restart。
- 不以宽泛 handler 静默吞掉所有 condition；保留调试信息和原始 cause。
- 使用 `unwind-protect` 或宏封装确保文件、流、锁和临时状态清理。
- 并发能力因实现而异；锁、线程、本地存储和原子操作必须通过项目选定库，不能假设可移植。
- 共享可变对象必须有所有权和同步模型；避免在持锁时调用未知 generic function。

## 文档与验证

- exported function、generic、class、condition 和 macro 提供 docstring，说明参数、返回值、副作用、condition 和线程语义。
- 注释解释 reader macro、声明、优化和实现特定限制，不复述 S-expression。
- 使用编译器 warning、style-warning、ASDF test-op 和项目测试；测试 package 边界、macro expansion、condition/restart 和不同优化级别。

来源：<https://google.github.io/styleguide/lispguide.xml>
