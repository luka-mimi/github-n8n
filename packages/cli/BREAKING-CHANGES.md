# n8n 重大变更

此列表显示了所有包含重大更改的版本以及如何升级。

## 1.102.0

### 变更内容？

`N8N_RUNNERS_ALLOW_PROTOTYPE_MUTATION` 标志已被 `N8N_RUNNERS_INSECURE_MODE` 替代。新标志禁用所有任务运行器的安全措施，旨在为那些重视与 `puppeteer` 等库兼容性而非安全性的用户提供一个应急措施。

### 何时需要采取行动？

如果您正在使用 `N8N_RUNNERS_ALLOW_PROTOTYPE_MUTATION` 标志，或者发现任务运行器当前不支持您依赖的外部模块，请考虑在自担风险的情况下设置 `N8N_RUNNERS_INSECURE_MODE=true`。

## 1.98.0

### 变更内容？

作为路由指标一部分的 `last_activity` 指标已更改为输出自上一个时间戳标签方法以来的 Unix 时间（以秒为单位）。标签方法可能导致 Prometheus 中的高基数，从而导致性能下降。

在使用表单节点时，对 `iframe`、`video` 和 `source` 标签的参数进行了更严格的限制。

### 何时需要采取行动？

如果您一直在从 n8n 实例（版本 1.81.0 及更新版本）中摄取路由指标，您应该分析 `last_activity` 指标如何影响您的 Prometheus 实例，并可能清理旧数据。未来的指标也将以不同的格式提供，这需要考虑在内。

如果您使用 `iframe`、`video` 或 `source` 标签，并且使用了此处未列出的属性，或者使用了既不是 `http` 也不是 `https` 的方案，您需要更新您的节点或工作流。

### 变更内容？

n8n 现在要求的最低 Node.js 版本为 v20。

### 何时需要采取行动？

如果您通过 npm 或 PM2 使用 n8n，或者您正在为 n8n 做贡献。

### 如何升级：

将 Node.js 版本更新到 v20 或更高版本。

## 1.83.0

### 变更内容？

表单节点不再允许自定义 HTML 的 `input` 字段类型，以防止恶意 JavaScript 被添加。

### 何时需要采取行动？

如果您之前在表单节点的自定义 HTML 中使用了 `input`。

## 1.82.1

### 变更内容？

表单节点不再允许自定义 HTML 的 `input` 字段类型，以防止恶意 JavaScript 被添加。

### 何时需要采取行动？

如果您之前在表单节点的自定义 HTML 中使用了 `input`。

## 1.81.3

### 变更内容？

表单节点不再允许自定义 HTML 的 `input` 字段类型，以防止恶意 JavaScript 被添加。

### 何时需要采取行动？

如果您之前在表单节点的自定义 HTML 中使用了 `input`。

## 1.65.0

### 变更内容？

通过环境变量 `QUEUE_RECOVERY_INTERVAL` 进行的队列轮询已被移除。

### 何时需要采取行动？

如果您设置了环境变量 `QUEUE_RECOVERY_INTERVAL`，请将其移除，因为它不再有任何效果。

## 1.63.0

### 变更内容？

1. 工作服务器默认绑定到 IPv6，现在默认绑定到 IPv4。
2. 工作服务器的 `/healthz` 以前基于数据库和 Redis 检查报告健康状态。现在无论数据库和 Redis 状态如何，都会报告健康状态，数据库和 Redis 检查是 `/healthz/readiness` 的一部分。

### 何时需要采取行动？

1. 如果在使用默认端口启动工作服务器时遇到端口冲突错误，请使用 `QUEUE_HEALTH_CHECK_PORT` 为工作服务器设置不同的端口。
2. 如果您依赖于数据库和 Redis 检查来获取工作健康状态，请改为检查 `/healthz/readiness` 而不是 `/healthz`。

## 1.57.0

### 变更内容？

`verbose` 日志级别已合并到 `debug` 日志级别中。

### 何时需要采取行动？

如果您设置了环境变量 `N8N_LOG_LEVEL=verbose`，请将日志级别更新为 `N8N_LOG_LEVEL=debug`。

## 1.55.0

### 变更内容？

环境变量 `N8N_BLOCK_FILE_ACCESS_TO_N8N_FILES` 现在也阻止访问 n8n 的静态缓存目录 `~/.cache/n8n/public`。

### 何时需要采取行动？

如果您通过节点在 n8n 的静态缓存目录中读写文件，例如 `从磁盘读取/写入文件`，请更新您的节点以使用不同的路径。

## 1.52.0

### 变更内容？

通过 `N8N_METRICS_INCLUDE_DEFAULT_METRICS` 和 `N8N_METRICS_INCLUDE_API_ENDPOINTS` 启用的 Prometheus 指标已修复为包含默认的 `n8n_` 前缀。

### 何时需要采取行动？

如果您使用这些类别的 Prometheus 指标并使用非空前缀，请更新这些指标以匹配其新的前缀名称。

## 1.47.0

### 变更内容？

调用 `$(...).last()`（或 `$(...).first()` 或 `$(...).all()`）不带参数时，返回连接两个节点的输出的最后一项（或第一项或所有项）。之前返回的是该节点的第一个输出的项/项。

### 何时需要采取行动？

如果您在具有多个输出的节点（例如 `If`、`Switch`、`Compare Datasets` 等）中不带参数使用 `$(...).last()`（或 `$(...).first()` 或 `$(...)all()`），并希望其默认为第一个输出。在这种情况下，将其更改为 `$(...).last(0)`（或 `first` 或 `all`）。

这不影响数组函数 `[].last()`、`[].first()`。

## 1.40.0

### 变更内容？

环境变量 `DB_POSTGRESDB_USER` 的默认值从 `root` 切换为 `postgres`。

### 何时需要采取行动？

如果您的 Postgres 连接依赖于 `DB_POSTGRESDB_USER` 环境变量的旧默认值 `root`，您现在必须在环境中显式设置 `DB_POSTGRESDB_USER` 为 `root`。

## 1.37.0

### 变更内容？

`execute` CLI 命令的 `--file` 标志已被移除。

### 何时需要采取行动？

如果您的脚本依赖于 `execute` CLI 命令的 `--file` 标志，请更新它们以首先导入工作流，然后使用 `--id` 标志执行它。

## 1.32.0

### 变更内容？

n8n 身份验证 cookie 现在默认设置了 `Secure` 标志。

### 何时需要采取行动？

如果您在 `localhost` 以外的域上运行 n8n 而没有 HTTP**S**，您需要设置 HTTPS，或者可以通过将环境变量 `N8N_SECURE_COOKIE` 设置为 `false` 来禁用安全标志。

## 1.27.0

### 变更内容？

执行模式 `own` 被移除。
如果 `EXECUTIONS_PROCESS` 设置为 `main` 或配置文件中的 `executions.process` 设置为 `main`，n8n 将打印警告，但正常启动。
如果 `EXECUTIONS_PROCESS` 设置为 `own` 或配置文件中的 `executions.process` 设置为 `own`，n8n 将打印错误消息并拒绝启动。

### 何时需要采取行动？

如果您使用 `own` 模式并需要隔离和性能提升，请考虑使用队列模式，否则通过删除环境变量或配置字段切换到主模式。
如果您设置了环境变量 `EXECUTIONS_PROCESS` 或配置字段 `executions.process`，请将其移除。环境变量不再有效，配置字段将在未来版本中被移除，防止 n8n 启动。

## 1.25.0

### 变更内容？

如果主实例上的环境变量 `N8N_ENCRYPTION_KEY` 与配置文件中的 `encryptionKey` 不匹配，主实例将无法初始化。如果工作节点上缺少环境变量 `N8N_ENCRYPTION_KEY`，工作节点将无法初始化。

### 何时需要采取行动？

如果将环境变量 `N8N_ENCRYPTION_KEY` 传递给主实例，请确保其与配置文件中的 `encryptionKey` 匹配。如果您使用工作节点，请将环境变量 `N8N_ENCRYPTION_KEY` 传递给它们。

## 1.24.0

### 变更内容？

标志 `N8N_CACHE_ENABLED` 被移除。缓存现在始终启用。

此外，凭据中的表达式现在遵循配对项，因此如果您有多个输入项，n8n 将尝试配对匹配的行以填写凭据详细信息。

在 Monday.com 节点中，由于 API 更改，`column_values` 数组中条目的数据结构已更改

### 何时需要采取行动？

如果您使用标志 `N8N_CACHE_ENABLED`，请将其从设置中移除。

关于凭据，如果您在凭据中使用表达式，您可能需要重新审视它们。以前，n8n 只会坚持第一个项目，但现在它将尝试匹配正确的配对项目。

如果您使用 Monday.com 节点并引用 `column_values` 属性，请在下表中检查您是否使用了其条目的任何受影响属性。

| 资源   | 操作           | 之前        | 新                 |
| ---------- | ------------------- | --------------- | ------------------- |
| Board      | Get                 | owner           | owners              |
| Board      | Get All             | owner           | owners              |
| Board Item | Get                 | title           | column.title        |
| Board Item | Get All             | title           | column.title        |
| Board Item | Get By Column Value | title           | column.title        |
| Board Item | Get                 | additional_info | column.settings_str |
| Board Item | Get All             | additional_info | column.settings_str |
| Board Item | Get By Column Value | additional_info | column.settings_str |

\*column.settings_str 不是 additional_info 的完整等价物

## 1.22.0

### 变更内容？

哈希算法 `ripemd160` 从 `.hash()` 表达式中删除。
`sha3` 哈希算法现在返回有效的 sha3-512 哈希，而不是以前返回的 `Keccak` 哈希。

### 何时需要采取行动？

如果您在表达式中使用哈希算法 `ripemd160` 的 `.hash` 助手，您需要切换到其他支持的算法之一。

## 1.15.0

### 变更内容？

到目前为止，在主模式下，n8n 在关闭时取消注册 webhook，并在启动时重新注册它们。队列模式和标志 `N8N_SKIP_WEBHOOK_DEREGISTRATION_SHUTDOWN` 跳过 webhook 注销。

从现在开始，在主模式和队列模式下，n8n 不再在启动和关闭时注销 webhook，并且标志 `N8N_SKIP_WEBHOOK_DEREGISTRATION_SHUTDOWN` 被移除。n8n 假设第三方服务将重试未处理的 webhook 请求。

### 何时需要采取行动？

如果使用标志 `N8N_SKIP_WEBHOOK_DEREGISTRATION_SHUTDOWN`，请注意它不再有效，可以从设置中移除。

## 1.9.0

### 变更内容？

在节点中，`this.helpers.getBinaryStream()` 现在是异步的。

### 何时需要采取行动？

如果您的节点使用 `this.helpers.getBinaryStream()`，请在调用时添加 `await`。

示例：

```typescript
const binaryStream = this.helpers.getBinaryStream(id); // 直到 1.9.0
const binaryStream = await this.helpers.getBinaryStream(id); // 自 1.9.0 起
```

### 变更内容？

环境变量 `N8N_BINARY_DATA_TTL` 和 `EXECUTIONS_DATA_PRUNE_TIMEOUT` 不再有任何效果，可以安全地移除。n8n 目前在修剪期间与执行一起清理二进制数据，而不是依赖于二进制数据的 TTL 系统。

### 何时需要采取行动？

如果使用这些标志，请将它们从设置中移除，并注意新的行为。

## 1.6.0

### 变更内容？

环境变量 `N8N_PERSISTED_BINARY_DATA_TTL` 不再有任何效果，可以移除。此遗留标志最初是为了支持临时执行而引入的（请参阅 [详细信息](https://github.com/n8n-io/n8n/pull/7046)），但不再支持。

### 何时需要采取行动？

如果使用此标志，请将其从设置中移除。

## 1.5.0

### 变更内容？

在代码节点中，`console.log` 默认不输出到 stdout。

### 何时需要采取行动？

如果您依赖于代码节点的非手动执行的 `console.log`，您需要将环境变量 `CODE_ENABLE_STDOUT` 设置为 `true`，以将代码节点日志发送到进程的 stdout。

## 1.2.0

### 变更内容？

对于 Linear 节点，问题创建中的优先级为 `4`（以前错误地为 `3`）表示 `Low`。

### 何时需要采取行动？

如果您使用 `Low`，您设置的优先级为 `Normal`，请仔细检查您设置的优先级是否符合您的意图。

## 1.0.0

### 变更内容？

n8n 现在要求的最低 Node.js 版本为 v18。

### 何时需要采取行动？

如果您通过 npm 或 PM2 使用 n8n，或者您正在为 n8n 做贡献。

### 如何升级：

将 Node.js 版本更新到 v18 或更高版本。

## 0.234.0

### 变更内容？

此版本引入了两个不可逆的更改：

- n8n 数据库将使用字符串而不是数值来标识工作流和凭据
- 执行数据被拆分到一个单独的数据库表中

### 何时需要采取行动？

无法使用旧版本的 n8n 读取 n8n@0.234.0 数据库，因此我们建议您在迁移前进行完整备份。

## 0.232.0

### 变更内容？

由于 Node.js/OpenSSL 升级，以下加密算法不再受支持。

- RSA-MD4
- RSA-MDC2
- md4
- md4WithRSAEncryption
- mdc2
- mdc2WithRSA

### 何时需要采取行动？

如果您在任何工作流中的 Crypto 节点中使用上述任何加密算法，请将节点中的算法属性更新为支持的值之一。

### 变更内容？

`LoneScale List` 节点已重命名为 `LoneScale`。

### 何时需要采取行动？

如果您在任何工作流中使用了 `LoneScale List` 节点。

### 如何升级：

更新任何使用 `LoneScale List` 的工作流以使用更新的节点。

## 0.226.0

### 变更内容？

`extractDomain` 和 `isDomain` 现在也匹配 localhost、没有协议的域和带有查询参数的域。
`extractUrl` 和 `isUrl` 还匹配 localhost 和带有查询参数的域。

### 何时需要采取行动？

如果您使用 `extractDomain` 或 `isDomain` 函数，并期望它们不匹配 localhost、没有协议的域和带有查询参数的域。

## 0.223.0

### 变更内容？

n8n 现在要求的最低 Node.js 版本为 v16。

### 何时需要采取行动？

如果您通过 npm 或 PM2 使用 n8n，或者您正在为 n8n 做贡献。

### 如何升级：

将 Node.js 版本更新到 v16 或更高版本。

## 0.214.0

### 变更内容？

无效的 Luxon 日期时间不再解析为 `null`。现在它们抛出错误 `invalid DateTime`。

### 何时需要采取行动？

如果您依赖于上述行为，请检查您的工作流以确保您处理无效的 Luxon 日期时间。

## 0.202.0

### 变更内容？

从 NPM 切换到 PNPM 进行开发。

### 何时需要采取行动？

如果您正在为 n8n 做贡献。

### 如何升级：

确保您的本地开发设置已更新为最新的 [贡献指南](../../CONTRIBUTING.md)。

## 0.198.0

### 变更内容？

合并节点操作列表已重新排列。

### 何时需要采取行动？

如果您使用了经过大修的合并节点和"按字段合并"、"按位置合并"或"多路复用"操作。

### 如何升级：

转到使用合并节点的工作流，选择"组合"操作，然后从"组合模式"中选择与以前使用的操作匹配的选项。如果您想在错误时继续，可以将"继续失败"设置为 true。

## 0.171.0

### 变更内容？

当响应包含错误时，GraphQL 节点现在会出错。

### 何时需要采取行动？

如果您使用 GraphQL 节点。

### 如何升级：

转到使用 GraphQL 节点的工作流并调整它们以适应新行为。如果您想在错误时继续，可以将"继续失败"设置为 true。

## 0.165.0

### 变更内容？

当"忽略 SSL 问题"选项设置为 False 时，Hive 节点现在会正确拒绝无效的 SSL 证书。

### 何时需要采取行动？

如果您使用自签名证书与 Hive。

### 如何升级：

转到 Hive 的凭据，启用"忽略 SSL 问题"选项。

## 0.139.0

### 变更内容？

对于 HubSpot 触发器节点，身份验证过程已更改为 OAuth2。

### 何时需要采取行动？

如果您使用 Hubspot 触发器。

### 如何升级：

在 HubSpot 中创建一个应用程序，使用客户端 ID、客户端密钥、应用程序 ID 和开发者密钥，并完成 OAuth2 流程。

## 0.135.0

### 变更内容？

凭据和二进制数据的节点内核方法已更改。

### 何时需要采取行动？

如果您使用自定义 n8n 节点。

### 如何升级：

1. 方法 `this.getCredentials(myNodeCredentials)` 现在是异步的。因此，必须在其前面添加 `await`。

示例：

```typescript
// 0.135.0 之前：
const credentials = this.getCredentials(credentialTypeName);

// 自 0.135.0 起：
const credentials = await this.getCredentials(myNodeCredentials);
```

2. 不应再直接访问二进制数据，而是必须使用方法 `await this.helpers.getBinaryDataBuffer(itemIndex, binaryPropertyName)`。

示例：

```typescript
const items = this.getInputData();

for (const i = 0; i < items.length; i++) {
	const item = items[i].binary as IBinaryKeyData;
	const binaryPropertyName = this.getNodeParameter('binaryPropertyName', i);
	const binaryData = item[binaryPropertyName] as IBinaryData;
	// 0.135.0 之前：
	const binaryDataBuffer = Buffer.from(binaryData.data, BINARY_ENCODING);
	// 自 0.135.0 起：
	const binaryDataBuffer = await this.helpers.getBinaryDataBuffer(i, binaryPropertyName);
}
```

## 0.131.0

### 变更内容？

对于 Pipedrive 常规节点，`deal:create` 操作现在需要组织 ID 或人员 ID，以符合即将对 Pipedrive API 的更改。

### 何时需要采取行动？

如果您在 Pipedrive 常规节点中使用 `deal:create` 操作，请设置组织 ID 或人员 ID。

## 0.130.0

### 变更内容？

对于 Taiga 常规和触发器节点，服务器和云凭据类型现在统一为单一凭据类型，并且 `version` 参数已被移除。此外，`issue:create` 操作现在会自动将标签加载为 `multiOptions`。

### 何时需要采取行动？

如果您使用 Taiga 节点，请重新连接凭据。如果您在 `issue:create` 操作中使用标签，请重新选择它们。

## 0.127.0

### 变更内容？

对于 Zoho 节点，`lead:create` 操作现在需要"公司"参数，"地址"参数现在位于"附加选项"中，"标题"和"是否重复记录"参数被移除。此外，`lead:delete` 操作现在仅返回已删除线索的 `id`。

### 何时需要采取行动？

如果您使用 `lead:create` 并带有"公司"或"地址"，请重置参数；对于其他两个参数，无需采取行动。如果您使用 `lead:delete` 的响应，请重新选择 `id` 键。

## 0.118.0

### 变更内容？

n8n 现在要求的最低 Node.js 版本为 v14。

### 何时需要采取行动？

如果您通过 npm 或 PM2 使用 n8n，或者您正在为 n8n 做贡献。

### 如何升级：

将 Node.js 版本更新到 v14 或更高版本。

---

### 变更内容？

在 Postgres、CrateDB、QuestDB 和 TimescaleDB 节点中，`Execute Query` 操作返回所有执行的查询的结果，而不仅仅是一个结果。

### 何时需要采取行动？

如果您使用上述任何节点的 `Execute Query` 操作，并且结果对您很重要，建议您重新审视您的逻辑。节点输出现在可能包含比以前更多的信息。此更改是为了使 n8n 的行为更加一致，其中具有多行输入的输入应根据所有输入数据产生结果，而不仅仅是一个。请注意：n8n 已经根据输入运行多个查询。只有输出被更改。

## 0.117.0

### 变更内容？

"激活触发器"节点已被移除。此节点已被两个其他节点替换。

"激活触发器"节点在版本 0.113.0 中添加，但未完全符合 UX，因此我们决定尽快重构和更改它，以便影响最少的用户。

新节点是"n8n 触发器"和"工作流触发器"。在行为上，节点是相同的，我们只是拆分了功能以使其对用户更直观。

### 何时需要采取行动？

如果您在任何工作流中使用"激活触发器"，请将其替换为新节点。

### 如何升级：

删除以前的节点，并根据您的工作流添加新节点。

---

更改了使用 Postgres Wire Protocol 的节点的行为：Postgres、QuestDB、CrateDB 和 TimescaleDB。

所有节点都已标准化，现在遵循相同的模式。对于大多数情况，行为将相同，但现在可以探索新添加的功能。

您现在还可以告知您希望 n8n 如何执行查询。默认模式是"多个查询"，这与以前的行为相同，但您现在可以独立或事务地运行它们。此外，"继续失败"现在在新模式中起着重要作用。

`insert` 操作的节点输出现在依赖于新参数 `Return fields`，就像以前的 `update` 操作一样。

### 何时需要采取行动？

如果您依赖于任何上述节点的 `insert` 操作返回的输出，我们建议您检查您的工作流。

默认情况下，所有 `insert` 操作将具有 `Return fields: *` 作为默认设置，返回所有插入的信息。

以前，节点会返回它接收到的所有信息，而不考虑数据库中实际发生的情况。

## 0.113.0

### 变更内容？

在 Dropbox 节点中，两个凭据类型（访问令牌和 OAuth2）都有一个名为"APP 访问类型"的新参数。

### 何时需要采取行动？

如果您使用具有权限类型"应用程序文件夹"的 Dropbox 应用程序。

### 如何升级：

打开您的 Dropbox 节点的凭据，并将"APP 访问类型"参数设置为"应用程序文件夹"。

## 0.111.0

### 变更内容？

在 Dropbox 节点中，现在所有操作都是相对于用户的根目录执行的。

### 何时需要采取行动？

如果您使用任何资源/操作进行 OAuth2 身份验证。

如果您在 Dropbox 帐户中使用 `folder:list` 操作，并且参数 `Folder Path` 为空（根路径），并且您在 Dropbox 帐户中有一个团队空间。

### 如何升级：

打开 Dropbox 节点，转到您正在使用的 OAuth2 凭据并重新连接它。

此外，如果您使用 `folder:list` 操作，请确保您的逻辑考虑到响应中的团队文件夹。

## 0.105.0

### 变更内容？

在 Hubspot 触发器中，现在可以提供多个事件，并且字段 `App ID` 被移动到凭据中。

### 何时需要采取行动？

如果您使用 Hubspot 触发器节点。

### 如何升级：

打开 Hubspot 触发器并重新设置事件。还打开凭据 `Hubspot Developer API` 并设置您的 APP ID。

## 0.104.0

### 变更内容？

不再支持 MongoDB 作为 n8n 的数据库，因为 MongoDB 在文档中保存大量数据时存在问题，以及其他问题。

### 何时需要采取行动？

如果您一直在使用 MongoDB 作为 n8n 的数据库。请注意，这与 MongoDB 节点无关。

### 如何升级：

在升级之前，您可以使用 CLI [导出](https://docs.n8n.io/hosting/cli-commands/#export-workflows-and-credentials) 您的所有凭据和工作流。

```
n8n export:workflow --backup --output=backups/latest/
n8n export:credentials --backup --output=backups/latest/
```

然后，您可以将数据库更改为 [此处](https://docs.n8n.io/hosting/configuration/supported-databases-settings) 提到的受支持数据库之一。最后，您可以升级 n8n 并将所有凭据和工作流 [导入](https://docs.n8n.io/hosting/cli-commands/#import-workflows-and-credentials) 回 n8n。

```
n8n import:workflow --separate --input=backups/latest/
n8n import:credentials --separate --input=backups/latest/
```

## 0.102.0

### 变更内容？

- `As User` 属性和 `User Name` 字段被合并并重命名为 `Send as User`。它也被移动到"添加选项"下。
- `Ephemeral` 属性被移除。要发送临时消息，您必须选择"发布（临时）"操作。

### 何时需要采取行动？

如果您在 Slack 节点中使用以下字段或属性：

- As User
- Ephemeral
- User Name

### 如何升级：

打开 Slack 节点并重新设置它们为适当的值。

---

### 变更内容？

如果您在 Typeform 中有一个问题使用先前回答的问题作为其文本的一部分，Typeform 触发器节点中的问题文本将如下所示：

`您选择了 {{field:23234242}} 作为您的答案。这样对吗？`

这些大括号破坏了表达式编辑器。更改使其现在显示如下：

`您选择了 [field:23234242] 作为您的答案。这样对吗？`

### 何时需要采取行动？

如果您使用 Typeform 触发器节点，并使用 [回忆信息](https://help.typeform.com/hc/en-us/articles/360050447072-What-is-Recall-information-) 功能的问题。

### 如何升级：

在使用 Typeform 触发器节点的工作流中，引用此类键名（使用先前回答的问题作为其文本的一部分的问题）的节点需要更新。

## 0.95.0

### 变更内容？

在 Harvest 节点中，我们将帐户字段从凭据移动到节点参数。这将允许您在不必创建多个凭据的情况下使用多个帐户。

### 何时需要采取行动？

如果您使用 Harvest 节点。

### 如何升级：

打开节点设置参数 `Account ID`。

## 0.94.0

### 变更内容？

在 Segment 节点中，我们更改了如何定义属性"traits"和"properties"。现在，可以提供键/值对，允许您发送自定义特征/属性。

### 何时需要采取行动？

当设置了属性"traits"或"properties"时，并且使用了以下资源/操作之一：

| 资源 | 操作 |
| -------- | --------- |
| Identify | Create    |
| Track    | Event     |
| Track    | Page      |
| Group    | Add       |

### 如何升级：

打开受影响的资源/操作并重新设置参数"traits"或"properties"。

## 0.93.0

### 变更内容？

更改了 Pipedrive 触发器节点的身份验证字段的命名。

### 何时需要采取行动？

如果您为节点中的"身份验证"字段设置了"基本身份验证"。

### 如何升级：

"身份验证"字段已重命名为"传入身份验证"。请将参数"传入身份验证"设置为"基本身份验证"以重新激活它。

## 0.90.0

### 变更内容？

运行 n8n 需要 Node.js 版本 12.9 或更高版本。

### 何时需要采取行动？

如果您运行的 Node.js 版本低于 12.9。

### 如何升级：

您可以从 [此处](https://nodejs.org/en/download/) 下载并安装最新版本的 Node.js。

## 0.87.0

### 变更内容？

link.fish 节点被移除，因为服务即将关闭。

### 何时需要采取行动？

如果您正在积极使用 link.fish 节点。

### 如何升级：

不幸的是，这是不可能的。我们建议您寻找替代服务。

## 0.83.0

### 变更内容？

在 Active Campaign 节点中，我们更改了 `getAll` 操作如何与各种资源一起工作，以保持一致性。为此，添加了一个名为"Simple"的新参数。

### 何时需要采取行动？

当使用以下资源/操作之一时：

| 资源                  | 操作 |
| ------------------------- | --------- |
| Deal                      | Get All   |
| Connector                 | Get All   |
| E-commerce Order          | Get All   |
| E-commerce Customer       | Get All   |
| E-commerce Order Products | Get All   |

### 如何升级：

打开受影响的资源/操作并将参数 `Simple` 设置为 false。

## 0.79.0

### 变更内容？

我们重命名了 Todoist 节点中的操作，以与代码库保持一致。我们还删除了 `close_match` 和 `delete_match` 操作，因为这些操作可以使用以下操作完成：`getAll`、`close` 和 `delete`。

### 何时需要采取行动？

当使用以下操作之一时：

- close_by
- close_match
- delete_id
- delete_match

### 如何升级：

升级后，打开包含 Todoist 节点的所有工作流。设置相应的操作，然后保存工作流。

如果使用了 `close_match` 或 `delete_match` 操作，请使用操作：`getAll`、`delete` 和 `close` 重新创建它们。

## 0.69.0

### 变更内容？

我们简化了 Twitter 节点处理附件的方式。您现在可以通过单击"添加字段"并选择"附件"来添加附件，而不是单击"添加附件"并必须指定"类别"。不再有选项来指定您添加的附件类型。

### 何时需要采取行动？

如果您在 Twitter 节点中使用了附件选项。

### 如何升级：

您需要为 Twitter 节点重新创建附件。

## 0.68.0

### 变更内容？

为了更容易使用 Slack 节点输出的数据，如果唯一的其他属性是 `ok": true`。在这种情况下，它现在直接返回"channel"下的数据。

### 何时需要采取行动？

当您当前使用 Slack 节点进行操作通道 -> 创建并使用节点输出的任何数据时。

### 如何升级：

所有引用的值之前在"channel"属性下的值现在都在主级别。这意味着这些表达式必须进行调整。

这意味着如果之前使用的表达式是：

```
{{ $node["Slack"].data["channel"]["id"] }}
```

它必须更改为：

```
{{ $node["Slack"].data["id"] }}
```

## 0.67.0

### 变更内容？

以下节点的名称未正确设置，现已修复：

- AMQP 发送器
- Bitbucket-触发器
- Coda
- Eventbrite-触发器
- Flow
- Flow-触发器
- Gumroad-触发器
- Jira
- Mailchimp-触发器
- PayPal 触发器
- 读取 PDF
- Rocketchat
- Shopify
- Shopify-触发器
- Stripe-触发器
- Toggl-触发器

### 何时需要采取行动？

如果在任何工作流中使用了上述节点。

### 如何升级：

对于上述节点，您需要通过打开凭据并将其从"无访问"移动到"访问"来再次授予它们对凭据的访问权限。完成此操作后，有两种方法可以升级工作流并使其在新版本中工作：

**简单**

- 在升级之前记下节点的设置
- 升级后，从工作流中删除上述节点，并重新创建它们

**高级**

升级后，在编辑器中选择整个工作流，复制并粘贴到文本编辑器中。在 JSON 中，手动更改节点类型，替换"type"的值如下：

- "n8n-nodes-base.amqpSender" -> "n8n-nodes-base.amqp"
- "n8n-nodes-base.bitbucket" -> "n8n-nodes-base.bitbucketTrigger"
- "n8n-nodes-base.Coda" -> "n8n-nodes-base.coda"
- "n8n-nodes-base.eventbrite" -> "n8n-nodes-base.eventbriteTrigger"
- "n8n-nodes-base.Flow" -> "n8n-nodes-base.flow"
- "n8n-nodes-base.flow" -> "n8n-nodes-base.flowTrigger"
- "n8n-nodes-base.gumroad" -> "n8n-nodes-base.gumroadTrigger"
- "n8n-nodes-base.Jira Software Cloud" -> "n8n-nodes-base.jira"
- "n8n-nodes-base.Mailchimp" -> "n8n-nodes-base.mailchimpTrigger"
- "n8n-nodes-base.PayPal" -> "n8n-nodes-base.payPalTrigger"
- "n8n-nodes-base.Read PDF" -> "n8n-nodes-base.readPDF"
- "n8n-nodes-base.Rocketchat" -> "n8n-nodes-base.rocketchat"
- "n8n-nodes-base.shopify" -> "n8n-nodes-base.shopifyTrigger"
- "n8n-nodes-base.shopifyNode" -> "n8n-nodes-base.shopify"
- "n8n-nodes-base.stripe" -> "n8n-nodes-base.stripeTrigger"
- "n8n-nodes-base.toggl" -> "n8n-nodes-base.togglTrigger"

然后删除所有现有节点，然后将更改后的 JSON 直接粘贴到 n8n 中。它应该会重新创建所有节点和连接，这次是工作节点。

## 0.62.0

### 变更内容？

在函数和函数项节点中，函数"evaluateExpression(...)"被重命名为"$evaluateExpression()"，以简化代码并规范化函数名称。

### 何时需要采取行动？

如果在任何函数或函数项节点中使用"evaluateExpression(...)"。

### 如何升级：

只需将"evaluateExpression(...)"替换为"$evaluateExpression(...)"。

## 0.52.0

### 变更内容？

为了确保所有节点的工作方式相似，以便于从工作流的其他部分轻松使用值，并能够在表达式中手动构建源日期，节点必须更改。现在，值不再直接从流中获取，而是必须通过表达式手动设置。

### 何时需要采取行动？

如果您当前使用"日期和时间"节点。

### 如何升级：

打开"日期和时间"节点，并通过表达式引用应转换的日期。还要将"属性名称"设置为应设置转换日期的属性的名称。

## 0.37.0

### 变更内容？

为了支持 Rocketchat 本地部署，凭据必须更改。`subdomain` 参数必须重命名为 `domain`。

### 何时需要采取行动？

当您当前使用 Rocketchat 节点时。

### 如何升级：

打开 Rocketchat 凭据并填写参数 `domain`。如果您之前设置了子域"example"，现在必须设置为"https://example.rocket.chat"。

## 0.19.0

### 变更内容？

节点"从 URL 读取文件"已被移除，因为其功能已添加到"HTTP 请求"节点中。

### 何时需要采取行动？

如果在任何工作流中使用了"从 URL 读取文件"节点。

### 如何升级：

升级后，打开所有包含"从 URL 读取文件"节点的工作流。它们将有一个"？"作为图标，因为它们不再被识别。创建一个新的"HTTP 请求"节点以替换旧节点，并添加与以前节点相同的 URL（如果您不再知道它，可以选择旧节点，复制并粘贴到文本编辑器中，它将显示节点包含的所有数据）。然后将"响应格式"设置为"文件"。一切将像以前一样正常运行。

---

### 变更内容？

当"HTTP 请求"属性"响应格式"设置为"字符串"时，它默认将数据保存在属性"response"中。在新版本中，现在可以进行配置。默认值也从"response"更改为"data"，以匹配具有类似功能的其他节点。

### 何时需要采取行动？

当使用"响应格式"设置为"字符串"的"HTTP 请求"节点时。

### 如何升级：

升级后，打开所有包含相关节点的工作流，并将"二进制属性"设置为"response"。

## 0.18.0

### 变更内容？

由于拼写错误，代码中经常使用`reponse`而不是`response`。因此，在 Webhook 节点上也是如此。其参数`reponseMode`必须重命名为正确的拼写`responseMode`。

### 何时需要采取行动？

当使用"响应模式"设置为"最后一个节点"的 Webhook 节点时。

### 如何升级：

升级后，打开所有包含相关 Webhook 节点的工作流，并手动将"响应模式"重新设置为"最后一个节点"。

---

### 变更内容？

由于 n8n 使用的 CLI 库不再维护，并且包含安全漏洞的包，我们不得不切换到另一个库。

### 何时需要采取行动？

当您当前直接通过其 JavaScript 文件启动 n8n 时。例如：

```
/usr/local/bin/node ./dist/index.js start
```

### 如何升级：

将路径更改为其新位置：

```
/usr/local/bin/node bin/n8n start
```
