# AngularJS 开发规则

先读取 `core-engineering.md` 和 `javascript.md`。本规则只适用于现有 AngularJS 1.x 代码；不要在现代 Angular、React、Vue 等项目中新增 AngularJS，除非任务明确维护遗留边界。

## 模块与文件

- 每个文件围绕单一 component、controller、service、directive、filter 或 module 配置组织，并遵循项目命名和目录布局。
- 使用 module API 注册组件，不依赖全局变量；避免重新定义已有 module。
- dependency injection 必须在 minification 后安全，使用项目规定的显式 annotation 或构建插件。
- 模块依赖保持单向，避免循环引用和大而全 shared module。
- template、style 和 controller 是否共置服从现有架构，不借维护任务重排整个应用。

## Controller、Component 与 Directive

- controller 保持轻量，只做视图协调；业务规则、数据访问和可复用状态放入 service。
- 优先 component API 表达组件；维护 directive 时限制为 DOM/表现行为并提供清晰 isolate scope/binding。
- 使用 `controllerAs`/项目既有模式，避免在深层 scope 上隐式挂载和依赖 prototype inheritance。
- binding 明确单向输入、事件输出和可变对象所有权；禁止子组件暗中修改父级共享状态。
- link/compile 只在真实 DOM 生命周期需求时使用，并在 `$destroy` 清理 listener、watcher、timer 和第三方插件。

## Service、异步与 Digest

- service/factory 保持单一职责和明确 public API；不要把所有全局状态集中成万能 service。
- Promise 使用 `$q` 或项目兼容机制，保持 digest 集成；错误必须传播或在稳定边界处理。
- HTTP 调用集中处理认证、错误映射、取消和重试；不在 controller 重复拼装协议逻辑。
- 避免手工 `$apply`/`$digest`；第三方异步边界确需触发时检查当前 phase。
- watcher 数量和表达式成本受控；避免在 template 中调用昂贵函数和创建新对象。
- event broadcast 只用于清晰跨层事件；优先显式 binding/service，避免难追踪的全局事件总线。

## Template 与安全

- template 使用语义 HTML、稳定 track key 和清晰表达式；复杂逻辑移到 controller/service。
- `ng-repeat` 使用合适 `track by`，不要用不稳定 index 表示有身份的数据。
- 不对不可信内容使用 `ng-bind-html` 或信任绕过；必须经过上下文匹配的 sanitization。
- route/state 参数、URL、DOM 属性和服务端数据均视为不可信，在服务端继续验证授权。
- 保持无障碍 label、键盘、focus 和 live region 行为。

## 测试与维护

- 单元测试 controller/service/filter 的纯行为；directive/component 测试 binding、digest、事件与销毁清理。
- 异步测试使用框架 flush/tick 能力，不依赖真实 timeout 和网络。
- 运行 formatter、ESLint、template linter、unit/e2e test 和生产构建。
- 安全修复和依赖升级应评估 AngularJS 已停止主流支持的风险；大规模迁移需要单独计划，不作为顺手重构。

来源：<https://google.github.io/styleguide/angularjs-google-style.html>
