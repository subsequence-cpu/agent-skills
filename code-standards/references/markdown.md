# Markdown 文档规则

先读取 `core-engineering.md`。以仓库采用的 CommonMark、GitHub Flavored Markdown、文档生成器、formatter 和 Google Markdown Style Guide 为准。

## 文档结构

- 独立文档通常只有一个一级标题，并与文档主题一致；后续标题按层级递进，不跳级。
- 使用 ATX 标题 `#`，井号后留空格；不要混用 setext 标题，除非仓库明确采用。
- 标题简短、描述性且稳定，避免手工硬编码易失效的 anchor。
- 开头直接说明目的和适用对象；长文档提供必要目录，但不要为短文制造目录噪声。
- 将前置条件、步骤、结果、限制和后续操作组织为读者可扫描的结构。

## 行宽、段落与列表

- 普通正文尽量在 80 列左右换行；长 URL、表格、标题、代码和工具要求的内容可超出。
- 段落之间空一行；不要用连续空格或空行进行视觉布局。
- unordered list 使用 `-`；ordered list 使用一致数字策略；列表前后保留空行以确保渲染稳定。
- 同级列表缩进一致；嵌套列表按 parser 需要缩进，不用 Tab。
- 并列项保持语法结构一致；完整句子使用一致标点，短标签可不加句号。
- task list 仅用于真实可跟踪状态，不代替正式 issue 或测试结果。

## 代码、命令与链接

- 行内标识符、文件名、命令、参数和短代码使用反引号。
- 多行代码使用 fenced code block，并在已知时标注语言；代码围栏前后空一行。
- 命令示例区分输入与输出，避免把 prompt 字符复制进实际命令；secret 使用明显占位符。
- 链接文字应说明目标，不使用“这里”或裸 URL 作为普通正文；外链指向最接近支持内容的权威规范或官方文档。
- 图片提供有意义 alt；装饰图片使用空 alt，并遵循仓库路径和资源管理方式。
- reference-style 与 inline link 选择服从仓库一致性；不要留下未使用或失效 reference。

## 表格、HTML 与特殊结构

- 只有表格确实比列表更清晰时使用 Markdown table；单元格过长、含多段或复杂代码时改用列表/小节。
- 表格列无需手工像素级对齐，交给 formatter；确保 header separator 和 escape 正确。
- 优先 Markdown 原生语法，避免嵌入 HTML；仅在目标 renderer 不支持所需表达时使用最小 HTML。
- admonition、frontmatter、MDX/模板语法按所属生成器规则，不能假设在其他 renderer 可移植。
- blockquote 用于引用或引用式提示，不用作任意缩进；引用内容应遵守版权和来源要求。

## 内容质量与维护

- 使用主动、直接、可执行的语言；术语、大小写和产品名保持一致。
- 注释、示例和命令必须与当前代码/API 一致；修改行为时同步更新文档。
- 不复制大段容易过期的外部文档；摘要关键规则并链接对应的权威规范或官方文档。
- 相对链接必须从当前文件可解析；重命名标题或文件时检查 inbound link。
- 不在文档示例中放真实 token、账户、内部主机或个人数据。

## 验证

- 运行 Markdown formatter/linter、链接检查和站点构建。
- 检查目标 renderer 中的标题、列表、代码、表格、frontmatter、图片和内部链接。

来源：<https://google.github.io/styleguide/docguide/style.html>
