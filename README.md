# code-standards

面向 Codex 的中文代码规范 Skill。它将编程语言、标记与数据格式规则同前端、后端工程规则分离，并根据代码职责按需加载。语言风格主要参考 Google Style Guides；Java 规则另结合《阿里巴巴 Java 开发手册（黄山版）》进行操作性整理。

## 设计原则

- 优先执行仓库指令、格式化器、静态检查器、编译器和 CI 配置。
- 始终应用公共工程规则，仅加载当前任务涉及的语言、格式和工程职责规则。
- 根据代码职责判断前端或后端，不依据实现语言作固定推断。
- 将修改限制在任务范围内，避免无关格式化、现代化或架构调整。
- 正确性、安全性、兼容性和测试优先于纯格式偏好。

## 覆盖范围

| 类别 | 规范范围 |
| --- | --- |
| 编程语言 | Java、C++、C#、Go、Objective-C、Swift、Kotlin、Dart、Python、Shell/Bash、R、Common Lisp、Vim script、JavaScript、TypeScript |
| 标记与数据格式 | HTML、CSS、Sass、JSON、JSONC、XML、Markdown |
| 前端工程 | 组件与状态、数据获取、可访问性、浏览器安全、性能、构建、依赖和测试 |
| 后端工程 | API、身份与授权、事务、数据库、缓存、消息、可靠性、可观测性、发布和兼容性 |
| 框架补充 | AngularJS 1.x 遗留代码 |

## 规则加载方式

Skill 始终读取公共工程规则，再按任务加载其他参考文件：

1. 根据源文件类型加载对应语言、标记或数据格式规则。
2. 涉及浏览器 UI、组件、路由、客户端状态、资源构建、可访问性或 Web 性能时，加载前端工程规则。
3. 涉及服务端 API、认证授权、持久化、事务、缓存、消息、后台任务、外部集成或可观测性时，加载后端工程规则。
4. 全栈任务同时加载前端和后端规则，并分别应用于所属边界。

典型组合如下：

| 任务 | 加载规则 |
| --- | --- |
| React、Vue 或现代 Angular 浏览器应用 | JavaScript/TypeScript、HTML/CSS、前端工程 |
| Node.js、Deno 或 Bun 服务 | JavaScript/TypeScript、后端工程及相关数据格式 |
| Java、Go、Python 或 C# 服务 | 对应语言、后端工程及相关数据格式 |
| 全栈功能 | 涉及的语言与格式、前端工程、后端工程 |
| AngularJS 1.x 维护 | JavaScript、AngularJS、前端工程 |

## 使用

将 `code-standards` 目录安装到 Codex 的用户 Skills 目录，然后在任务中显式调用：

```text
$code-standards 按项目适用的语言和前后端工程规范实现并审查这次修改。
```

也可以针对具体职责提出要求：

```text
$code-standards 按 TypeScript 与前端工程规范审查这个表单流程。
$code-standards 按 Java 与后端工程规范实现这个幂等 API。
```

## 规则优先级

发生冲突时按以下顺序处理：

1. 用户明确要求和仓库指令；
2. 格式化器、静态检查器、编译器和 CI 配置；
3. 不影响正确性或安全性的现有局部约定；
4. 本 Skill 中适用的语言和工程规则。

## 目录结构

```text
code-standards/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── core-engineering.md
    ├── frontend-engineering.md
    ├── backend-engineering.md
    ├── sources.md
    ├── java.md
    ├── javascript.md
    ├── typescript.md
    └── 其他语言与格式规范.md
```

## 规范参考

完整的语言、格式及前后端工程规范参考见 [`code-standards/references/sources.md`](code-standards/references/sources.md)。
