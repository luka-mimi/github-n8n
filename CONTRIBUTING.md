# 贡献于 n8n

很高兴您在这里并希望为 n8n 做出贡献

## 目录

- [贡献于 n8n](#贡献于-n8n)
	- [目录](#目录)
	- [行为准则](#行为准则)
	- [目录结构](#目录结构)
	- [开发设置](#开发设置)
		- [开发容器](#开发容器)
		- [要求](#要求)
			- [Node.js](#nodejs)
			- [pnpm](#pnpm)
				- [pnpm 工作区](#pnpm-工作区)
			- [corepack](#corepack)
			- [构建工具](#构建工具)
		- [实际 n8n 设置](#实际-n8n-设置)
		- [开始](#开始)
	- [开发周期](#开发周期)
		- [社区 PR 指南](#社区-pr-指南)
			- [**1. 变更请求/评论**](#1-变更请求评论)
			- [**2. 一般要求**](#2-一般要求)
			- [**3. PR 特定要求**](#3-pr-特定要求)
			- [**4. 不合规 PR 的工作流程总结**](#4-不合规-pr-的工作流程总结)
		- [测试套件](#测试套件)
			- [单元测试](#单元测试)
			- [代码覆盖率](#代码覆盖率)
			- [E2E 测试](#e2e-测试)
	- [发布](#发布)
	- [创建自定义节点](#创建自定义节点)
	- [扩展文档](#扩展文档)
	- [贡献工作流模板](#贡献工作流模板)
	- [贡献者许可协议](#贡献者许可协议)

## 行为准则

本项目及其所有参与者均受行为准则的约束，该准则可在文件 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 中找到。参与即表示您同意遵守此准则。请将不可接受的行为报告给 jan@n8n.io。

## 目录结构

n8n 被分成不同的模块，所有模块都在一个单一的 mono 仓库中。

最重要的目录：

- [/docker/images](/docker/images) - 创建 n8n 容器的 Dockerfiles
- [/packages](/packages) - 不同的 n8n 模块
- [/packages/cli](/packages/cli) - 运行前端和后端的 CLI 代码
- [/packages/core](/packages/core) - 处理工作流执行、活动 webhooks 和工作流的核心代码。**在此处进行任何更改之前请联系 n8n**
- [/packages/frontend/@n8n/design-system](/packages/design-system) - Vue 前端组件
- [/packages/frontend/editor-ui](/packages/editor-ui) - Vue 前端工作流编辑器
- [/packages/node-dev](/packages/node-dev) - 创建新 n8n 节点的 CLI
- [/packages/nodes-base](/packages/nodes-base) - 基础 n8n 节点
- [/packages/workflow](/packages/workflow) - 前端和后端使用的接口的工作流代码

## 开发设置

如果您想更改或扩展 n8n，您必须确保安装了所有需要的依赖项并正确链接了包。以下是如何完成此操作的简短指南：

### 开发容器

如果您已经安装了 VS Code 和 Docker，您可以点击[这里](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/n8n-io/n8n)开始。点击这些链接将导致 VS Code 自动安装开发容器扩展（如果需要），将源代码克隆到容器卷中，并启动一个开发容器以供使用。

### 要求

#### Node.js

[Node.js](https://nodejs.org/en/) 版本 22.16 或更高版本是开发所需的。

#### pnpm

[pnpm](https://pnpm.io/) 版本 10.2 或更高版本是开发所需的。我们建议使用 [corepack](#corepack) 安装。

##### pnpm 工作区

n8n 被分成不同的模块，所有模块都在一个单一的 mono 仓库中。为了便于模块管理，使用了 [pnpm 工作区](https://pnpm.io/workspaces)。这会自动在相互依赖的模块之间设置文件链接。

#### corepack

我们建议使用 `corepack enable` 启用 [Node.js corepack](https://nodejs.org/docs/latest-v16.x/api/corepack.html)。

您可以使用 `corepack prepare --activate` 安装正确版本的 pnpm。

**重要**：如果您通过 homebrew 安装了 Node.js，您需要运行 `brew install corepack`，因为 homebrew 明确从 [node 公式](https://github.com/Homebrew/homebrew-core/blob/master/Formula/node.rb#L66) 中删除了 `npm` 和 `corepack`。

**重要**：如果您在 Windows 上，您需要以管理员身份在终端中运行 `corepack enable` 和 `corepack prepare --activate`。

#### 构建工具

n8n 使用的包依赖于一些构建工具：

Debian/Ubuntu：

```
apt-get install -y build-essential python
```

CentOS：

```
yum install gcc gcc-c++ make
```

Windows：

```
npm add -g windows-build-tools
```

MacOS：

不需要额外的包。

### 实际 n8n 设置

> **重要**：以下所有步骤至少要执行一次才能使开发设置正常运行！

现在 n8n 运行所需的一切都已安装，可以检出并设置实际的 n8n 代码：

1. [Fork](https://guides.github.com/activities/forking/#fork) n8n 仓库。

2. 克隆您 fork 的仓库：

   ```
   git clone https://github.com/<your_github_username>/n8n.git
   ```

3. 进入仓库文件夹：

   ```
   cd n8n
   ```

4. 将原始 n8n 仓库添加为 `upstream` 到您 fork 的仓库：

   ```
   git remote add upstream https://github.com/n8n-io/n8n.git
   ```

5. 安装所有模块的所有依赖项并将它们链接在一起：

   ```
   pnpm install
   ```

6. 构建所有代码：
   ```
   pnpm build
   ```

### 开始

要启动 n8n，请执行：

```
pnpm start
```

要使用隧道启动 n8n：

```
./packages/cli/bin/n8n start --tunnel
```

## 开发周期

在迭代 n8n 模块代码时，您可以运行 `pnpm dev`。它将自动构建您的代码，重启后端并在您进行的每次更改时刷新前端（editor-ui）。

1. 在开发模式下启动 n8n：
   ```
   pnpm dev
   ```
1. Hack, hack, hack
1. 检查一切是否仍在生产模式下运行：
   ```
   pnpm build
   pnpm start
   ```
1. 创建测试
1. 运行所有[测试](#测试套件)：
   ```
   pnpm test
   ```
1. 提交代码并[创建一个 pull 请求](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)

---

### 社区 PR 指南

#### **1. 变更请求/评论**

请在 14 天内解决请求的更改或提供反馈。如果在此期间没有响应或更新 pull 请求，它将自动关闭。应用请求的更改后可以重新打开 PR。

#### **2. 一般要求**

- **遵循样式指南：**
  - 确保您的代码符合 n8n 的编码标准和约定（例如，格式、命名、缩进）。在适用的情况下使用 linting 工具。
- **TypeScript 合规性：**
  - 不要使用 `ts-ignore`。
  - 确保代码符合 TypeScript 规则。
- **避免重复代码：**
  - 尽可能重用现有组件、参数和逻辑，而不是重新定义或复制它们。
  - 对于节点：在多个操作中使用相同的参数，而不是为每个操作定义一个新参数（如果适用）。
- **测试要求：**
  - PR **必须包含测试**：
    - 单元测试
    - 节点的工作流测试（示例[这里](https://github.com/n8n-io/n8n/tree/master/packages/nodes-base/nodes/Switch/V3/test)）
    - UI 测试（如果适用）
- **拼写错误：**
  - 使用拼写检查工具，例如 [**Code Spell Checker**](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)，以避免拼写错误。

#### **3. PR 特定要求**

- **仅限小型 PR：**
  - 每个 PR 专注于一个功能或修复。
- **命名约定：**
  - 遵循 [n8n 的 PR 标题约定](https://github.com/n8n-io/n8n/blob/master/.github/pull_request_title_conventions.md#L36)。
- **新节点：**
  - 引入新节点的 PR 将被**自动关闭**，除非它们是由 n8n 团队明确请求的并与商定的项目范围一致。然而，您仍然可以探索[构建您自己的节点](https://docs.n8n.io/integrations/creating-nodes/)，因为 n8n 提供了创建自定义节点的灵活性。
- **仅限拼写错误的 PR：**
  - 拼写错误不足以成为 PR 的理由，将被拒绝。

#### **4. 不合规 PR 的工作流程总结**

- **无测试：** 如果未提供测试，PR 将在 **14 天** 后自动关闭。
- **非小型 PR：** 大型或多方面的 PR 将被退回以进行分段。
- **新节点/拼写错误 PR：** 如果不符合项目范围或指南，将自动拒绝。

---

### 测试套件

#### 单元测试

可以通过以下方式启动单元测试：

```
pnpm test
```

如果在某个包文件夹中执行，它将只运行该包的测试。如果在 n8n 根文件夹中执行，它将运行所有包的所有测试。

如果您进行了需要更新 `.test.ts.snap` 文件的更改，请在运行测试时传递 `-u` 参数，或在监视模式下按 `u`。

#### 代码覆盖率
我们在 [Codecov](https://app.codecov.io/gh/n8n-io/n8n) 上跟踪所有代码的覆盖率。但当您在本地进行测试时，我们建议在设置环境变量 `COVERAGE_ENABLED` 为 `true` 的情况下运行测试。然后，您可以在 `coverage` 文件夹中查看代码覆盖率，或者可以使用 [这个 VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=ryanluker.vscode-coverage-gutters) 直接在 VSCode 中可视化覆盖率。

#### E2E 测试

⚠️ 您必须在第一次运行测试之前运行 `pnpm cypress:install` 以安装 cypress 并更新 cypress。

E2E 测试可以通过以下命令之一启动：

- `pnpm test:e2e:ui`：启动 n8n 并使用构建的 UI 代码交互式运行 e2e 测试。不响应代码更改（即运行 `pnpm start` 和 `cypress open`）
- `pnpm test:e2e:dev`：在开发模式下启动 n8n 并交互式运行 e2e 测试。响应代码更改（即运行 `pnpm dev` 和 `cypress open`）
- `pnpm test:e2e:all`：启动 n8n 并无头运行 e2e 测试（即运行 `pnpm start` 和 `cypress run --headless`）

⚠️ 请记得先停止您的开发服务器。否则端口绑定将失败。

## 发布

要开始发布，请使用 SemVer 发布类型触发[此工作流](https://github.com/n8n-io/n8n/actions/workflows/release-create-pr.yml)，并选择要从中剪切此发布的分支。此工作流将：

1. 提升已更改或具有已更改依赖项的包的版本
2. 更新变更日志
3. 创建一个名为 `release/${VERSION}` 的新分支，并
4. 创建一个新的 pull 请求以跟踪需要包含在此发布中的任何进一步更改

准备好发布后，只需合并 pull 请求即可。这将触发[另一个工作流](https://github.com/n8n-io/n8n/actions/workflows/release-publish.yml)，该工作流将：

1. 构建并发布此发布中具有新版本的包
2. 从压缩的发布提交中创建一个新的标签和 GitHub 发布
3. 将压缩的发布提交合并回 `master`

## 创建自定义节点

了解有关[构建节点](https://docs.n8n.io/integrations/creating-nodes/)的信息，以创建 n8n 的自定义节点。您可以创建社区节点并使用 [npm](https://www.npmjs.com/) 使其可用。

## 扩展文档

[n8n 文档](https://docs.n8n.io) 的仓库可以在[这里](https://github.com/n8n-io/n8n-docs)找到。

## 贡献工作流模板

您可以将您的工作流提交到 n8n 的模板库。

n8n 正在开发一个创作者计划，并开发一个模板市场。这是一个正在进行的项目，细节可能会发生变化。

请参阅 [n8n 创作者中心](https://www.notion.so/n8n/n8n-Creator-hub-7bd2cbe0fce0449198ecb23ff4a2f76f) 以获取有关如何提交模板和成为创作者的信息。

## 贡献者许可协议

为了避免将来出现任何潜在问题，不幸的是，签署[贡献者许可协议](CONTRIBUTOR_LICENSE_AGREEMENT.md)是必要的。这实际上可以通过按下一个按钮来完成。

我们使用了最简单的协议。它来自 [Indie Open Source](https://indieopensource.com/forms/cla)，使用简单的英语，实际上只有几行长。

一旦打开 pull 请求，自动机器人将立即留下评论请求签署协议。只有在获得签名后，pull 请求才能合并。
