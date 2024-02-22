# Web 项目工程实践

## CI/CD

CI/CD 应该是必备工具。

## 代码风格、lint、review

人员水平各不相同，在这样一个团队中，要保持一致的代码风格其实是很困难的。一是 JS 太灵活，很难控制代码的风格；二是每个成员的喜好不同，就算启用了 lint 工具，但总有一些人连基本的插件也不安装。

除非强制在 CI 时进行 lint 控制，但这样的就需要在工期中留有更多处理代码问题的时间。

至于 review，对于新人 review 毫毛作用。新人对于代码质量没有概念。对于老人，输出 review 大概属于自己写一半个需求。另一点就是很多开发很“珍惜”自己的代码，不到最后也不会提交到 CI 中，然后就是坨代码，根本无法 review。

## ls-lint

代码风格不一致，那是代码文件内部的问题。连文件名也无法保持一致命名风格。新人属于那种抄也抄不明白的状态，连模仿也搞不定。

## typescript

类型是一个好东西，整个项目强制使用 typescript 也是可以的，但我并不鼓励。让 javascript 开发转为写 typescript 最终只会变为 anyscript。与其使用 anyscript，不如直接使用 javascript。我遇到的很多 javascript 开发，甚至 java 开发对于类型的理解仅限于**类型 == class**。要明白，很多人从事开发，只是因为这是一份比较好的工作。

我鼓励在项目的公共组件中使用 typescript，在业务页面中直接使用 javascript。理由很简单，业务页面的稳定性远低于公共组件。当然公共的函数、类也属于公共组件。

## 编程规范

编程规范是指导的指南，实际意义不如直接使用 lint 工具。最理解的情况下应该是在 CI/CD 中增加门禁，lint 不通过就不能合并代码。当然不是说是编写编程规范不合理，而是对于大多数团队，直接使用 lint 工具的推荐规范成本是更少。毕竟编写规范也不是一两个新手能完成的事。不如“拿来主义”。

## 实用范式

### 组件库

组件库，例如 element-ui 之类，在项目早期会提升效率，可以快速交作业。不过很多时间每个项目或公司都会有自己的 UI 设计风格。选用的组件库如果风格与公司主流（UCD）风格不同时，组件库基本上属于除了基础的交互外，是用途不大的东西。在我项目，连 el-button 都需要处理过才能使用。所以非 UI 绑定的组件库可能有一定市场。

我认为更好的做法应该是尽可能使用原生的 html 来处理排版，然后组合自己的场景二次包装 element-ui 的基础组件（按钮之类）。一些复杂的表单输入组件，如果功能上、交互无法满足需求，应该优先考虑自己实现。要知道，一个专用的组件，其实并不是很难实现。element-ui 之类的组件库，因为要考虑通用性，会有非常多的功能。

当前一些复杂的组件，比如表格、图表，应该优先考虑三方实现。一是这些组件实现非常复杂，二是自己实现的版本，很容易会出现性能问题。

另一个重要的点是，很多场景通过业务设计，可以避免一些复杂的交互逻辑。使用复杂的交互会增加用户的学习成本，也会增加实现的难度。

### 事件

**何时触发 change 事件**

设置 modelValue 后，再 emit change 事件。

**事件命名**

参考 HTML 标准中的命令习惯，onclick、oninput、onchange、onresize 等，事件应该是 onXxxYyy 风格，xxx 是动词。

### 基于 Hook 的弹窗函数

弹窗是经常使用的组件。其实 drawer、dialog、message box 之类都可以是基于相同的底层实现。vue 推荐使用基于模板的弹窗实现，比如 element-ui 中的弹窗实现。但很多场景需要动态生成窗口内容或弹出不同的窗口，如果可以通过 openDialog() 这样的函数就可以打开一个窗口会更实用。比如 [NgModalService](https://ng.ant.design/components/modal/zh#components-modal-demo-service)

### 表格

表格是经常使用的数据展示组件。表格尽量不要自己实现，表格的交互、计算很多不推荐自己整个轮子。但是应该基于组件库包装一个表格。可以考虑两种包装：

1. 轻量包装，只封装常用的参数，在项目中可以快速实现类似的表格
2. 基于上面的轻量包装，搞出一个声明式的表格，可以包含分页，通用操作之类的，尽可能可以直接用于实现业务页面

### 交互风格

项目需要确定基本交互逻辑。可以考虑一下这几个问题：

1. 页面之间如何切换？是 SPA 的层层切换，还是直接打开新窗口？
2. 某个资源的详情数据使用哪种形式展示？
   - 跳转到新页面？
   - 弹窗展示？
     - 弹窗的话直接使用大窗口，还是使用 drawer
     - 使用弹窗，如果有二层的页签，如何处理？
3. 更新或创建数据时，如何处理？
4. 用户登录如何处理？
   使用 SSO？
   如果使用 SSO，用户的权限又如何处理？

## 后台

接口是 restful 的，200 总是表格操作一切正常，无业务出错。400 表示由于前端数据导致的问题，500 表示后台内部导致的问题。
