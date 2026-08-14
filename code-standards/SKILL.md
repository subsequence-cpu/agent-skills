---
name: code-standards
description: 基于 Google 官方代码风格指南执行实用的开发语言与工程规范，并使用《阿里巴巴 Java 开发手册（黄山版）》扩展 Java 工程规则。编写、修改、重构、调试或审查源代码时使用；新增 API、测试、依赖、数据库访问、并发、日志或安全敏感行为时使用；检查 Java、C++、C#、Go、Objective-C、Swift、Kotlin、Dart、Python、Shell、R、Common Lisp、Vim script、JavaScript、TypeScript、HTML/CSS、JSON、XML、Markdown 或 AngularJS 的命名、格式、惯用写法和可维护性时使用。
---

# 开发语言工程规范

优先执行仓库中可自动验证的规范，再应用本 Skill 对应的语言规则。将修改限制在任务范围内，同时验证行为正确性与代码风格。

## 加载适用规则

始终阅读 [references/core-engineering.md](references/core-engineering.md)，然后仅加载与当前文件或行为相关的逐语言规则：

- Java：读取 [references/java.md](references/java.md)
- C++：读取 [references/cpp.md](references/cpp.md)
- C#：读取 [references/csharp.md](references/csharp.md)
- Go：读取 [references/go.md](references/go.md)
- Objective-C：读取 [references/objective-c.md](references/objective-c.md)
- Swift：读取 [references/swift.md](references/swift.md)
- Kotlin：读取 [references/kotlin.md](references/kotlin.md)
- Dart：读取 [references/dart.md](references/dart.md)
- Python：读取 [references/python.md](references/python.md)
- Shell/Bash：读取 [references/shell.md](references/shell.md)
- R：读取 [references/r.md](references/r.md)
- Common Lisp：读取 [references/common-lisp.md](references/common-lisp.md)
- Vim script：读取 [references/vim-script.md](references/vim-script.md)
- JavaScript：读取 [references/javascript.md](references/javascript.md)
- TypeScript：读取 [references/typescript.md](references/typescript.md)
- HTML/CSS/Sass：读取 [references/html-css.md](references/html-css.md)
- JSON/JSONC：读取 [references/json.md](references/json.md)
- XML：读取 [references/xml.md](references/xml.md)
- Markdown：读取 [references/markdown.md](references/markdown.md)
- AngularJS：同时读取 [references/angularjs.md](references/angularjs.md) 和 [references/javascript.md](references/javascript.md)
- 需要核对来源、处理未支持语言或确认规范是否更新：读取 [references/sources.md](references/sources.md)

涉及多种语言时，加载所有相关规则，不要加载无关内容。

## 执行流程

1. 编辑前检查仓库指令、格式化器、静态检查器、编译配置、构建文件、相邻代码和测试。
2. 确认语言版本、运行时约束、公共兼容边界、生成代码边界和风险区域。
3. 按以下优先级解决规则冲突：
   1. 用户明确要求和仓库指令；
   2. 格式化器、静态检查器、编译器和 CI 配置；
   3. 不影响正确性或安全性的现有局部约定；
   4. 本 Skill 中对应的语言规则。
4. 实现最小且完整的改动，不要顺带格式化或现代化无关文件。
5. 除非任务明确要求改变，否则保持行为和兼容性；为改变的行为新增或更新测试。
6. 依次运行范围最小且相关的格式化、静态检查、测试和构建命令，并根据风险扩大验证范围。
7. 报告已运行的检查、未能运行的检查，以及任何有意偏离规范之处。

## 一致运用判断

- 正确性、安全性、数据完整性、并发、兼容性和测试缺口优先于纯格式问题。
- 优先依赖自动格式化和静态检查规则，不做主观的手工风格调整。
- 审查代码时提供可执行的发现和文件、行号位置；不要为了满足清单而编造问题。
- 若代码库明确采用另一套成熟规范，遵循该规范，仅在兼容处应用本 Skill。
- 对未支持语言，遵循仓库工具和该语言当前的一手官方指南，并使用公共工程规则约束跨语言问题。

## 处理 Java 格式重叠

Java 始终以仓库工具为最高优先级。新项目未声明格式规范时，采用 Google Java 默认格式，包括两空格块缩进和 100 列限制；并使用黄山版衍生规则处理类型、金额、时间、集合、并发、异常、日志、安全、API、测试、MySQL、ORM、依赖和分层设计。
