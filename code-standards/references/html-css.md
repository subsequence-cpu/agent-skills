# HTML/CSS/Sass 开发规则

先读取[公共工程规则](core-engineering.md)和[前端工程规则](frontend-engineering.md)。本文件只处理 HTML、CSS 与 Sass 的语义和样式规则；组件架构、状态、浏览器安全、性能和前端测试由前端工程规则处理。以项目 formatter、Stylelint、HTML validator、框架模板语法和 Google HTML/CSS Style Guide 为准。

## 通用格式与元信息

- 使用 HTTPS 引用可用的图片、媒体、样式和脚本，不省略协议。
- 使用无 BOM 的 UTF-8；HTML 文档通过 `<meta charset="utf-8">` 指定编码。
- 没有 formatter 时使用两空格缩进、不使用 Tab、删除行尾空格；代码标记、属性、selector、property 和非字符串 value 使用小写。
- 注释解释结构、兼容限制和为何采用方案；TODO 使用项目认可且可追踪的格式。
- 格式化源文件而非生成、压缩或编译产物；修改 Sass/GSS 时在源层完成。

## HTML 结构与语义

- 文档以 `<!doctype html>` 进入 no-quirks mode，并尽可能通过标准校验。
- 使用元素本身语义：heading、paragraph、button、link、list、table 等不得全部替换为 div。
- 图片提供准确 alt；纯装饰图片使用空 alt；音视频提供 caption/transcript 等替代访问。
- 保持 heading 层级、landmark、label、name/role/value、键盘和 focus 行为，优先原生语义而非补 ARIA。
- 分离结构、表现和行为；避免 inline style 和 inline event handler，除非框架明确要求。
- UTF-8 可直接表达的字符不使用 entity，只有 HTML 特殊字符和不可见控制字符例外。
- HTML5 CSS/JavaScript 资源省略无必要 `type`；可选 tag 是否省略由项目一致决定。
- styling 优先 class，script hook 优先 data attribute；避免无必要 id，确需 id 时保证唯一和可预测。

## HTML 格式与安全

- block、list、table element 通常各占一行并缩进子元素；属性换行由 formatter 统一。
- attribute value 使用双引号；boolean attribute 使用项目/框架规范形式。
- template 输出默认转义；不可信内容不得直接进入 raw HTML、URL、style、event 或 script context。
- 外部链接、iframe、form 和资源遵循 CSP、sandbox、rel、referrer、integrity 等项目安全策略。
- 不以隐藏元素或视觉顺序破坏 DOM 阅读顺序和键盘导航。

## CSS 命名与选择器

- class 名表达功能或组件语义，不使用颜色、位置等纯表现名称，也不使用晦涩缩写。
- 多词 class 使用连字符；大型/嵌入式项目可使用短且唯一的应用前缀或既有 BEM/模块策略。
- 避免 ID selector、过深 descendant selector、无必要 element+class 限定和过高 specificity。
- component 样式限制作用域；使用既有 design token、自定义属性和层级策略，不引入第二套命名系统。
- 避免 `!important`、UA sniffing 和 CSS hack；仅在明确第三方/遗留边界局部使用并记录。

## CSS 声明与格式

- 可正确表达时使用 shorthand，但不能因 shorthand 意外重置未涉及属性。
- 0 通常不带单位；小于 1 的数写前导 0；可缩写的 hex color 可用三位形式，项目 token 优先。
- declaration 每行一个并以分号结束；property colon 后一个空格，opening brace 与 selector 同行。
- 多 selector 每行一个；rule 之间空一行；property 顺序服从 formatter/Stylelint，没有工具时一致或按字母排序。
- CSS 字符串和 attribute selector 使用单引号；`url()` 通常不加引号，特殊内容与项目工具例外。
- Sass nesting 保持浅且反映组件关系；mixin、extend、function 和变量只解决真实复用，不制造难追踪输出。

## 验证

- 运行 formatter、HTML/CSS validator、Stylelint、模板编译和视觉/无障碍测试。
- 检查响应式布局、主题、缩放、RTL、键盘、screen reader、浏览器支持和 CSS 产物大小。

来源：<https://google.github.io/styleguide/htmlcssguide.html>
