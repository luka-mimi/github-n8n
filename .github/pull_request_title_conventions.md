# PR 标题约定

我们对 Pull Requests（到 `master` 分支）的格式有非常精确的规则。此格式基本上遵循 [Angular 提交信息约定](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)。它使提交历史更易于阅读，并允许自动生成发布说明：

PR 标题由以下元素组成：

```text
<type>(<scope>): <summary>
  │       │          │
  │       │          └─⫸ 概要：使用祈使句现在时。
  |       |                        首字母大写
  |       |                        末尾无句号。
  │       │
  │       └─⫸ 范围：API | benchmark | core | editor | * Node
  │
  └─⫸ 类型：build | ci | chore | docs | feat | fix | perf | refactor | test
```

- PR 标题
  - 类型
  - 范围（_可选_）
  - 概要
- PR 描述
  - 正文（可选）
  - 空行
  - 页脚（可选）

结构如下：

## 类型

必须是以下之一：

| 类型 | 描述 | 是否出现在变更日志 |
| --- | --- | --- |
| `feat` | 新功能 | ✅ |
| `fix` | 错误修复 | ✅ |
| `perf` | 提高性能的代码更改 | ✅ |
| `test` | 添加缺失的测试或更正现有测试 | ❌ |
| `docs` | 仅文档更改 | ❌ |
| `refactor` | 不改变行为的代码更改，既不修复错误也不添加功能 | ❌ |
| `build` | 影响构建系统或外部依赖项的更改（TypeScript、Jest、pnpm 等） | ❌ |
| `ci` | CI 配置文件和脚本的更改（例如 Github actions） | ❌ |
| `chore` | 常规任务、维护和其他类型未涵盖的小更新 | ❌ |

> 重大更改（见下文页脚部分），将**始终**出现在变更日志中，除非后缀为 `no-changelog`。

## 范围（可选）

只要提交明确解决以下支持的范围之一，范围应指定提交更改的位置。（否则，省略范围！）

- `API` - 对 _公共_ API 的更改
- `benchmark` - 对基准 cli 的更改
- `core` - 对 n8n 核心/私有 API/后端的更改
- `editor` - 对编辑器 UI 的更改
- `* Node` - 对特定节点或触发节点的更改（"*"应替换为节点名称，而不是其显示名称），例如
  - mattermost → Mattermost 节点
  - microsoftToDo → Microsoft To Do 节点
  - n8n → n8n 节点

## 概要

概要包含对更改的简洁描述：

- 使用祈使句现在时："更改"而不是"已更改"或"更改中"
- 首字母大写
- 末尾无点（.）
- 不包括 Linear 工单 ID 等（例如 N8N-1234）
- 对于不应在变更日志中提及的提交/PR，后缀为"(no-changelog)"。

## 正文（可选）

就像在 **概要** 中一样，使用祈使句现在时："更改"而不是"已更改"或"更改中"。正文应包括更改的动机，并与之前的行为进行对比。

## 页脚（可选）

页脚可以包含有关重大更改和弃用的信息，也是 [引用 GitHub 问题](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword)、Linear 工单和此提交关闭或相关的其他 PR 的位置。例如：

```text
BREAKING CHANGE: <重大更改摘要>
<空行>
<重大更改描述 + 迁移说明>
<空行>
<空行>
修复 #<问题编号>
```

或

```text
DEPRECATED: <弃用内容>
<空行>
<弃用描述 + 推荐更新路径>
<空行>
<空行>
关闭 #<pr 编号>
```

重大更改部分应以短语"`BREAKING CHANGE:`"开头，后跟重大更改的摘要、空行和重大更改的详细描述，其中还包括迁移说明。

> 💡 重大更改还可以通过在标题中添加"`!"`来标记，就在":"之前，例如 `feat(editor)!: 移除对暗模式的支持`
>
> 这使得在浏览提交信息时更容易定位重大更改。

> 💡 重大更改还必须添加到 n8n 仓库中的 [packages/cli/BREAKING-CHANGES.md](https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md) 文件中。

同样，弃用部分应以"`DEPRECATED:`"开头，后跟弃用内容的简短描述、空行和弃用的详细描述，其中还提到推荐的更新路径。

### 撤销提交

如果提交撤销了先前的提交，则应以 `revert:` 开头，后跟被撤销提交的标题。

提交信息正文的内容应包含：

- 关于被撤销提交的 SHA 的信息，格式如下：`This reverts commit <SHA>`，
- 清楚描述撤销提交信息的原因。
