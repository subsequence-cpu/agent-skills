# 规范参考与更新策略

本 Skill 提供可执行的规则摘要，不能替代规范原文。语言文件主要转述对应风格指南；前端、后端工程文件综合多项标准和工程实践，不表示每条建议均由单一规范直接规定。需要精确措辞、处理未覆盖边界情况或确认规范版本时，应查阅对应规范原文并与仓库配置协调。

## 语言与格式规范参考

- Google Style Guides 索引：<https://google.github.io/styleguide/>
- C++：<https://google.github.io/styleguide/cppguide.html>
- C#：<https://google.github.io/styleguide/csharp-style.html>
- Go：<https://google.github.io/styleguide/go/>
- HTML/CSS：<https://google.github.io/styleguide/htmlcssguide.html>
- JavaScript：<https://google.github.io/styleguide/jsguide.html>
- Java：<https://google.github.io/styleguide/javaguide.html>
- JSON：<https://google.github.io/styleguide/jsoncstyleguide.xml>
- Markdown：<https://google.github.io/styleguide/docguide/style.html>
- Objective-C：<https://google.github.io/styleguide/objcguide.html>
- Python：<https://google.github.io/styleguide/pyguide.html>
- R：<https://google.github.io/styleguide/Rguide.html>
- Shell：<https://google.github.io/styleguide/shellguide.html>
- Swift：<https://google.github.io/swift/>
- TypeScript：<https://google.github.io/styleguide/tsguide.html>
- AngularJS：<https://google.github.io/styleguide/angularjs-google-style.html>
- Common Lisp：<https://google.github.io/styleguide/lispguide.xml>
- Vim script：<https://google.github.io/styleguide/vimscriptguide.xml>
- XML 文档格式：<https://google.github.io/styleguide/xmlstyle.html>
- Effective Dart：<https://dart.dev/effective-dart>
- Kotlin：<https://developer.android.com/kotlin/style-guide>
- 阿里巴巴 p3c 官方仓库：[《Java 开发手册（黄山版）》](https://github.com/alibaba/p3c/blob/master/Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%28%E9%BB%84%E5%B1%B1%E7%89%88%29.pdf)，作为 Java/JVM 工程规则参考。

对未支持语言，优先执行仓库工具和该语言维护方发布的现行官方规范。

## 前端与后端工程规范参考

- HTML Living Standard：<https://html.spec.whatwg.org/>
- WCAG 2.2：<https://www.w3.org/TR/WCAG22/>
- Web Vitals：<https://web.dev/articles/vitals>
- HTTP Semantics（RFC 9110）：<https://www.rfc-editor.org/rfc/rfc9110.html>
- OpenAPI Specification：<https://spec.openapis.org/oas/>
- OWASP Application Security Verification Standard：<https://owasp.org/projects/asvs>
- OpenTelemetry Specification：<https://opentelemetry.io/docs/specs/>
- Google Site Reliability Engineering：<https://sre.google/sre-book/table-of-contents/>

本节所列规范最后核对于 2026-09-18。需要精确合规时，应重新确认持续更新型规范的当前版本；引用具体 OWASP ASVS 要求时应包含版本号。

## 更新策略

1. 查阅外部指南前先检查仓库工具。
2. 规范可能变化或任务要求精确合规时，使用最新官方来源。
3. 将上游示例视为语言指导，不视为全仓库重新格式化的授权。
4. 保持 Java 冲突处理原则：仓库工具优先；否则语言层采用 Google 格式和黄山版衍生 Java/JVM 约束，服务端行为加载独立后端工程规则。
5. 根据代码职责而不是实现语言选择前端或后端规则。
6. 上游规则发生实质变化时更新摘要、核对日期和来源链接。

## 署名

Google Style Guides 使用 Creative Commons Attribution 3.0 许可。本 Skill 对选定规则进行转述并链接至官方指南。Java/JVM 工程规则依据阿里巴巴 p3c 官方仓库发布的《Java 开发手册（黄山版）》进行操作性整理，不能替代规范原文。
