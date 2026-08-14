# R 开发规则

先读取 `core-engineering.md`。Google R 指南基于 Tidyverse Style 并列出 Google 差异；以仓库 lintr、styler、renv 和既有 base-R/tidyverse 约定为准。

## 命名与格式

- Google 风格的函数使用 `BigCamelCase`，私有函数以点开头，如 `.DoWork`；若仓库明确采用 snake_case，则保持项目一致。
- 普通对象使用描述性名称，不使用旧式 `dot.case`，以免与 S3 method 混淆。
- 使用 `<-` 左赋值，禁止 `->` 右赋值；函数内部显式 `return()` 表达返回意图。
- 运算符两侧、逗号后加空格；使用项目 formatter 统一缩进、换行和 pipe 布局。
- 不依赖 partial argument matching；重要调用使用具名参数。

## 依赖、包与文档

- 禁止 `attach()`；显式引用数据和环境，防止搜索路径污染。
- 外部函数通常使用 `package::function()`，避免宽泛 `@import` 导致名称冲突。
- infix function、特定 rlang pronoun 和默认包等官方例外按项目 NAMESPACE 管理。
- 需要 import 时，将 `@importFrom` 放在实际使用依赖的函数 Roxygen header 中。
- 每个 package 提供 `packagename-package.R` 包级文档，并记录公共函数、参数、返回、错误和副作用。
- 依赖版本通过 renv/项目 lockfile 管理，不在脚本运行时随意安装 package。

## 数据与函数

- 函数职责集中，输入数据框的列、类型、因子/字符、缺失值、维度和单位要显式验证。
- 不隐式修改 `.GlobalEnv`、options、working directory、locale 或随机种子；确需修改时保存并恢复。
- 使用 `on.exit(..., add = TRUE)` 或项目机制清理连接、图形设备、临时文件和全局设置。
- vectorization 能提升清晰度时使用；复杂索引、recycling 和隐式 coercion 应拆开并验证。
- 注意 NA、NaN、Inf、NULL 与长度零向量的不同语义；条件必须为确定的标量 boolean。
- 大型数据操作明确复制、内存和计算成本；不能仅为管道简洁产生多次全量扫描。

## 错误、随机与测试

- 使用明确 condition/error，并在稳定边界增加上下文；不通过 `try(..., silent=TRUE)` 吞掉重要失败。
- 随机分析设置并记录种子；并行随机流使用可复现机制。
- 测试覆盖缺失值、空输入、单行/单列、因子 level、时区、浮点容差和数据顺序。
- 运行 styler、lintr、R CMD check、testthat 和文档生成检查；不得忽略新增 warning/note 而不解释。

来源：<https://google.github.io/styleguide/Rguide.html>
