# n8n 基准测试工具

用于对 n8n 实例执行基准测试的工具。

## 目录结构

```text
packages/@n8n/benchmark
├── scenarios        基准测试场景
├── src              n8n-benchmark CLI 的源代码
├── Dockerfile       n8n-benchmark CLI 的 Dockerfile
├── scripts          编排脚本
```

## 对现有 n8n 实例进行基准测试

运行现有基准测试场景的最简单方法是使用基准测试 Docker 镜像：

```sh
docker pull ghcr.io/n8n-io/n8n-benchmark:latest
# 打印帮助以列出所有可用标志
docker run ghcr.io/n8n-io/n8n-benchmark:latest run --help
# 以 5 个并发请求运行所有可用的基准测试场景 1 分钟
docker run ghcr.io/n8n-io/n8n-benchmark:latest run \
	--n8nBaseUrl=https://instance.url \
	--n8nUserEmail=InstanceOwner@email.com \
	--n8nUserPassword=InstanceOwnerPassword \
	--vus=5 \
	--duration=1m \
	--scenarioFilter=single-webhook
```

### 使用 Docker 镜像的自定义场景

也可以创建您自己的[基准测试场景](#benchmark-scenarios)并使用 `--testScenariosPath` 标志加载它们：

```sh
# 假设您的场景位于 `./scenarios`，将它们挂载到容器中的 `/scenarios`
docker run -v ./scenarios:/scenarios ghcr.io/n8n-io/n8n-benchmark:latest run \
	--n8nBaseUrl=https://instance.url \
	--n8nUserEmail=InstanceOwner@email.com \
	--n8nUserPassword=InstanceOwnerPassword \
	--vus=5 \
	--duration=1m \
	--testScenariosPath=/scenarios
```

## 运行整个基准测试套件

基准测试套件由[基准测试场景](#benchmark-scenarios)和不同的[n8n 设置](#n8n-setups)组成。

### 本地

```sh
pnpm benchmark-locally
```

### 在云中

```sh
pnpm benchmark-in-cloud
```

## 运行 `n8n-benchmark` CLI

`n8n-benchmark` CLI 是一个 node.js 程序，可以针对单个 n8n 实例运行一个或多个场景。

### 使用 Docker 本地运行

构建 Docker 镜像：

```sh
# 必须在存储库根目录中运行
# k6 没有可用于 linux 的 arm64 构建，我们需要针对 amd64 构建
docker build --platform linux/amd64 -t n8n-benchmark -f packages/@n8n/benchmark/Dockerfile .
```

运行镜像

```sh
docker run \
  -e N8N_USER_EMAIL=user@n8n.io \
  -e N8N_USER_PASSWORD=password \
  # 对于 macos，n8n 在 docker 外部运行
  -e N8N_BASE_URL=http://host.docker.internal:5678 \
  n8n-benchmark
```

### 不使用 Docker 本地运行

要求：

- [k6](https://grafana.com/docs/k6/latest/set-up/install-k6/)
- Node.js v20 或更高版本

```sh
pnpm build

# 使用指定的电子邮件和密码对 http://localhost:5678 运行测试
N8N_USER_EMAIL=user@n8n.io N8N_USER_PASSWORD=password ./bin/n8n-benchmark run
```

## 基准测试场景

基准测试场景定义了一个或多个要执行和测量的步骤。它由以下部分组成：

- 描述和配置场景的清单文件
- 在运行场景之前导入的任何测试数据
- 一个在运行时接收 `API_BASE_URL` 环境变量的 [`k6`](https://grafana.com/docs/k6/latest/using-k6/http-requests/) 脚本。

可用场景位于 [`./scenarios`](./scenarios/)。

## n8n 设置

n8n 设置定义了使用 Docker compose 的单个 n8n 运行时配置。不同的 n8n 设置位于 [`./scripts/n8nSetups`](./scripts/n8nSetups)。
