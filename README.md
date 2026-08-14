# code-standards

面向 Codex 的中文开发语言规范 Skill。它以 Google Style Guides 为主要语言风格来源，并用《阿里巴巴 Java 开发手册（黄山版）》补充 Java 工程规则。

## 覆盖范围

- Java、C++、C#、Go、Objective-C、Swift、Kotlin、Dart、Python、Shell、R、Common Lisp 和 Vim script
- JavaScript、TypeScript、HTML/CSS、JSON、XML、Markdown 和 AngularJS
- 跨语言的正确性、安全性、测试、依赖、日志、兼容性和可维护性要求

## 使用

将 `code-standards` 目录安装到 Codex 的用户 Skills 目录，然后在任务中使用：

```text
$code-standards 按项目适用的语言规范实现并审查这次修改。
```

Skill 会先遵循项目已有的仓库指令、格式化器、静态检查器和 CI 配置，再加载与当前文件相关的语言规则。详细来源见 [`code-standards/references/sources.md`](code-standards/references/sources.md)。

## 目录

```text
code-standards/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── core-engineering.md
    ├── sources.md
    └── 各语言规范.md
```

## 上游来源

- [Google Style Guides](https://google.github.io/styleguide/)
- 《阿里巴巴 Java 开发手册（黄山版）》（用户提供，用于 Java 工程规则的操作性改编）

上游规则及其内容适用各自的许可。本仓库中的转述不会替代上游原文。
