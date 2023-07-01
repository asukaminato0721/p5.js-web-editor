# 开发

添加代码到该项目的指南。

- [开发](#development)
  - [安装](#installation)
  - [开发流程](#development-workflow)
  - [测试](#tests)
  - [提交代码](#committing-code)
    - [一般规则](#general-rules)
    - [提交信息](#commit-messages)
  - [设计](#design)
  - [使用的技术](#technologies-used)

## 安装
请按照[安装指南](./installation.md)进行操作。

## 开发流程
* 该项目使用 git-flow。关于 git-flow 的详细概述，请阅读["一个成功的 Git 分支模型"](https://nvie.com/posts/a-successful-git-branching-model/)。
* [让我们停止使用 Master/Slave](https://medium.com/@mikebroberts/let-s-stop-saying-master-slave-10f1d1bf34df)

作为一个贡献代码但不创建生产版本的人（大多数人），以下是您需要了解的内容：
* 默认分支是 `develop`。所有的拉取请求都应该发送到该分支。它应该是稳定的，并且所有的提交在一个暂存服务器上可见。
* 在修复错误或添加功能时，您应该从 `develop` 分支创建一个分支。完成后，您应该从您的功能分支向 `develop` 发起一个拉取请求。
* `release` 分支是实时生产分支，也是部署到 editor.p5js.org 的代码。对该分支的更改应谨慎进行，并且将使用 git 标签完成。
* 紧急修复更改应该从 `release` 分支创建，并通过拉取请求合并到 `release`。在合并了拉取请求之后，提交可以合并到 `develop`。

有关创建发布版本的信息，请参阅[发布指南](./release.md)。

## 测试
要运行测试套件，只需运行 `npm test`（在使用 `npm install` 安装依赖之后）

这里有一个示例单元测试：[Nav.unit.test.jsx](../client/components/Nav.unit.test.jsx)。

## 提交代码
受 [Git/GitHub 提交标准与约定](https://gist.github.com/digitaljhelms/3761873) 的启发。

良好的提交信息至少具备以下三个重要目的：

* 加快审查过程。
* 帮助我们编写良好的发布说明。
* 帮助未来的维护者理解您的更改以及背后的原因。


### 一般规则
* 对于更改，请进行原子提交（即 [原子提交](http://en.wikipedia.org/wiki/Atomic_commit)），即使跨多个文件，在逻辑上也应当是一个整体。也就是说，尽可能确保每个提交都专注于一个特定的目的。
* 尽量确保提交不包含不必要的空格更改。可以使用以下命令检查：

```
$ git diff --check
```

### 提交信息

将提交信息按照以下结构编写：

 ```
 [#issueid] 50 字符或更少的更改摘要

 更详细的解释性文本，如果需要的话。将其换行到大约 72 个字符左右。在某些上下文中，第一行被视为电子邮件的主题，其余文本被视为正文。空行将摘要与正文分开是至关重要的（除非您完全省略正文）；如果将两者连在一起，像 rebase 这样的工具可能会感到困惑。

 进一步的段落在空行之后。

   - 也可以使用项目符号，例如：

   - 通常使用连字符或星号作为项目符号，前面加一个空格，并在它们之间加上空行，但约定在这方面有所不同
 ```
* 使用祈使句的形式编写摘要行和所做更改的描述，就好像您在给某人下命令一样。使用 "Fix"、"Add"、"Change" 开头，而不是 "Fixed"、"Added"、"Changed"。
* 在摘要行中用方括号括起来的形式链接您正在处理的 GitHub 问题，例如 [#123]。
* 始终留下第二行为空白。
* 在描述中尽可能详细地描述。这有助于推理提交的意图，并提供更多关于为何进行更改的上下文。
* 如果您发现很难总结您的提交所做的事情，可能是因为它包含了多个逻辑更改或错误修复，最好将其拆分为多个提交，使用 `git add -p`。
* 请注意，如果需要，可以将多个问题连接到一个提交中：`[#123][#456] Add Button component`

## 设计
- [Zeplin 上的样式指南/设计系统](https://scene.zeplin.io/project/55f746c54a02e1e50e0632c3)
- [Figma 上的最新设计](https://www.figma.com/file/5KychMUfHlq97H0uDsen1U/p5-web-editor-2017.p.copy?node-id=0%3A1)。请注意，网站上的当前设计可能已经分歧，部分设计将不会被实现，但仍然有助于参考。
- [移动端设计](https://www.figma.com/file/5KychMUfHlq97H0uDsen1U/p5-web-editor-2017.p.copy?node-id=0%3A2529)，[响应式设计](https://www.figma.com/file/5KychMUfHlq97H0uDsen1U/p5-web-editor-2017.p.copy?node-id=0%3A3292)

## 使用的技术

**MERN 栈** - MongoDB、Express、React/Redux 和 Node。

- 有关该项目使用的**文件结构格式**的参考，请查看[Mern Starter](https://github.com/Hashnode/mern-starter)。

- 该项目不使用 CSS Modules、styled-components 或其他 CSS-in-JS 库，而是使用 Sass。遵循[BEM 准则和命名约定](http://getbem.com/)。

- 对于常用且可重用的样式，请使用具有占位符和 mixin 的 OOSCSS（面向对象的 SCSS）进行编写。为了组织样式，请遵循[Sass 的 7-1 模式](https://sass-guidelin.es/#the-7-1-pattern)。

- 我们使用[ES6](http://es6-features.org/)并使用[Babel](https://babeljs.io/)将其转换为 ES5。

- 关于 JavaScript 的样式指南，请参阅[Airbnb 的样式指南](https://github.com/airbnb/javascript)、[React ESLint 插件](https://github.com/yannickcr/eslint-plugin-react)。

- ESLint 的配置基于一些常用的 React/Redux 脚手架。欢迎对此提出建议。如果在开发过程中，您对 ESLint 感到困扰，可以通过在行末添加注释 `// eslint-disable-line` 来禁用任何行。

- [Jest](https://jestjs.io/)用于单元测试和快照测试，同时使用[Enzyme](https://airbnb.io/enzyme/)来测试 React。
