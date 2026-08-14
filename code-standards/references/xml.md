# XML 文档与数据规则

先读取 `core-engineering.md`。以所属 XML Schema、namespace、解析器和 Google XML Document Format Style Guide 为准。

## 是否使用 XML

- 设计新格式前先评估已有标准能否满足互操作需求；不要只因熟悉而发明新 XML vocabulary。
- 若数据天然面向对象、配置或消息，比较 JSON、Protocol Buffers 等替代方案；需要 namespace、mixed content、Schema 或既有生态时 XML 更合适。
- 明确文档版本、扩展机制、兼容策略和规范所有者。

## 命名、namespace 与结构

- element、attribute、type 和 namespace 使用稳定一致的命名约定，不把显示文案作为机器标识。
- namespace URI 稳定且受控；prefix 只影响表示，不作为业务语义。
- 可重复、可扩展、具有子结构的数据优先 element；紧凑元数据、标识和修饰信息可用 attribute。
- 不把同一概念有时表示为 element、有时表示为 attribute；mixed content 只用于确实面向文本的格式。
- 顺序具有业务意义时在 Schema 和文档中明确；无意义时消费者不得依赖偶然顺序。
- ID、引用、枚举、日期、单位、空值和缺失值应有明确 Schema 语义。

## 格式与编码

- 使用 UTF-8；是否保留 XML declaration 服从协议。格式化源文档，不重排签名或工具生成内容。
- 没有 formatter 时使用两空格缩进、每层一个层级、属性换行一致且无行尾空格。
- 只为 XML 特殊字符使用 entity/escape；文本规范化、空白和 CDATA 语义必须清晰。
- comment 不得破坏机器处理，也不得包含 secret 或用于隐藏失效配置。
- Schema、XSLT、WSDL 等文件使用各自工具格式，不套用普通 XML 的主观布局。

## 安全与解析

- 对不可信输入禁用外部实体、外部 DTD 和不受控网络解析，防止 XXE 和实体膨胀。
- 限制文档大小、深度、attribute/element 数量和文本长度；解析器必须有超时和资源边界。
- 按 Schema 验证后仍要执行业务授权和跨字段不变量检查。
- XPath/XQuery 参数不得拼接不可信文本；namespace context 和返回基数要显式处理。
- XML Signature/Encryption 必须使用成熟库并防止签名包装、canonicalization 和节点选择错误。

## 演进与测试

- 新字段默认 optional 且旧消费者可忽略；破坏性结构变化使用版本 namespace 或明确迁移。
- 测试 namespace prefix 变化、未知 element/attribute、空白、编码、顺序、巨大输入和恶意实体。
- 使用 XML parser、Schema validator 和领域 consumer 做往返测试；签名文档验证 canonical form。

来源：<https://google.github.io/styleguide/xmlstyle.html>
