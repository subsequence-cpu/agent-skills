# JSON/JSONC 开发规则

先读取 `core-engineering.md`。以目标协议、JSON Schema、生成工具和 Google JSON Style Guide 为准；严格 JSON、JSONC、JSON5 不得混淆。

## 语法与格式

- 默认输出严格 JSON：property name 和 string 使用双引号，不允许 comment、尾随逗号、单引号、`NaN`、`Infinity` 或 `undefined`。
- 只有文件所有者明确声明 JSONC/JSON5 时才使用扩展语法，并由对应 parser/formatter 验证。
- 使用 UTF-8；缩进、换行和 key 排序服从所属工具。生成 manifest、lockfile 和大型 fixture 不手工重排。
- 顶层结构根据协议选择 object 或 array；配置和长期 API 通常优先 object 以便扩展。
- 数值必须在生产者、消费者共同安全范围内；超大整数、十进制金额和精确标识符使用 string 或明确编码。

## 属性与 Schema

- property name 应稳定、描述性且使用协议规定 casing；同一 API 不混用 snake_case 与 camelCase。
- 不使用保留、歧义或含实现细节的 key；单位在名称或 Schema 中明确。
- 区分 missing、`null`、空 string、空 array/object 和 0；Schema 与实现必须一致。
- 枚举 wire value 稳定且可向前兼容；消费者应按协议处理未知值。
- 时间使用明确标准格式与时区，通常为 RFC 3339；duration 和 epoch unit 必须声明。
- polymorphic object 使用稳定 discriminant，不通过猜测字段组合确定类型。
- 使用 JSON Schema/OpenAPI 等描述 required、type、format、range、pattern、additional properties 和版本兼容。

## 解析、安全与演进

- 外部 JSON 先限制总大小、深度、array 长度和 string 长度，再校验结构与业务权限。
- 不把 parse 成功视为可信；字段仍需类型、范围、格式和跨字段不变量验证。
- 防止 prototype pollution、重复 key 差异、数字精度损失和 parser 宽松模式造成的不一致。
- 日志和错误不得回显完整敏感 JSON；只记录必要且脱敏的定位字段。
- 新增 optional 字段保持旧消费者可用；删除、重命名、改变类型或含义需要版本与迁移策略。
- 生产者不要依赖 object key 顺序；若签名/哈希要求 canonical JSON，使用明确标准和实现。

## 验证

- 使用目标 parser、formatter 和 Schema validator 验证正反例。
- 测试 missing/null、unknown field、duplicate key、极端数字、Unicode、深层嵌套、超大输入和版本兼容。

来源：<https://google.github.io/styleguide/jsoncstyleguide.xml>
