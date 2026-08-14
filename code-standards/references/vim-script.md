# Vim script 开发规则

先读取 `core-engineering.md`。以项目支持的 Vim/Neovim 最低版本、legacy Vim script 或 Vim9 script 方言、formatter 和 Google Vim script Style Guide 为准。

## 文件与加载

- 文件放入正确的 plugin、autoload、ftplugin、syntax 或 after 目录，遵循运行时加载语义。
- 可重复加载的脚本使用适当 guard；ftplugin 修改选项、mapping 和 command 时设置可撤销的 `b:undo_ftplugin`。
- private helper 使用 script-local 作用域；autoload 函数使用与路径匹配的 `name#function`，避免启动期加载全部实现。
- 不修改与功能无关的用户全局选项、cwd、register、search、view 或 mapping；确需修改时保存并恢复。
- 同一文件不要混用 legacy 与 Vim9 语法，除非版本兼容层明确分离。

## 命名与格式

- function 和 variable 使用描述性名称；作用域前缀 `s:`、`b:`、`w:`、`g:`、`l:` 应准确。
- 公共 command、mapping、function 和 global variable 使用插件唯一前缀，防止运行时冲突。
- 常量、script-local 和 autoload 命名遵循项目一致约定，不使用含义不明缩写。
- 使用两空格或仓库规定缩进；continuation、dictionary、list 和 command 链由 formatter 统一。
- 复杂 expression 和 execute 字符串拆分；避免通过难以引用的字符串拼接生成命令。

## 函数、错误与状态

- legacy function 使用 `abort`，防止错误后继续执行；可被重复定义的开发代码按项目使用 `function!`。
- 参数、可选参数和返回语义保持简单；复杂配置使用 dictionary 并验证 key 与类型。
- 使用 `try`/`catch`/`finally` 处理可恢复错误和清理；不要通过宽泛 catch 隐藏真实异常。
- 调用外部 command 前处理 shell escaping；优先 list-form/job API，禁止拼接不可信命令。
- async job、timer、channel 和 callback 必须明确关闭、取消和 buffer 生命周期。
- mapping 使用合适的 `<silent>`、`<buffer>`、`<expr>` 与 non-recursive 形式，不覆盖用户按键而不提供退出策略。
- autocmd 放入唯一 augroup，并在重新定义前清理所属组，不能清理其他插件事件。

## 兼容性、文档与测试

- 所用函数、option、event 和 API 必须满足最低版本；需要时用 `exists()`/`has()` 局部兼容。
- 不因 Neovim 可用就把现有 Vim script 顺手迁移 Lua；迁移必须是明确项目决策。
- help 文档使用标准 tag 和章节格式；公共 command、mapping 和 option 说明副作用及默认值。
- 测试不同 buffer、window、tab、filetype、重复 source、缺失外部命令和异常清理。
- 运行项目 linter、headless Vim/Neovim 测试和支持版本矩阵。

来源：<https://google.github.io/styleguide/vimscriptguide.xml>
