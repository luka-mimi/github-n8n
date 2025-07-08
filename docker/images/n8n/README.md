![n8n.io - 工作流自动化](https://user-images.githubusercontent.com/65276001/173571060-9f2f6d7b-bac0-43b6-bdb2-001da9694058.png)

# n8n - 为技术团队提供安全的工作流自动化

n8n 是一个工作流自动化平台，为技术团队提供了代码的灵活性和无代码的速度。通过 400 多个集成、本地 AI 功能和公平代码许可，n8n 让您在保持对数据和部署的完全控制的同时构建强大的自动化。

![n8n.io - 截图](https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-screenshot-readme.png)

## 关键功能

- **需要时编写代码**：编写 JavaScript/Python，添加 npm 包，或使用可视化界面
- **AI 原生平台**：基于 LangChain 构建 AI 代理工作流，使用您自己的数据和模型
- **完全控制**：通过我们的公平代码许可自托管或使用我们的[云服务](https://app.n8n.cloud/login)
- **企业就绪**：高级权限、SSO 和隔离部署
- **活跃的社区**：400 多个集成和 900 多个现成的[模板](https://n8n.io/workflows)

## 目录

- [n8n - 工作流自动化工具](#n8n---工作流自动化工具)
  - [关键功能](#关键功能)
  - [目录](#目录)
  - [演示](#演示)
  - [可用集成](#可用集成)
  - [文档](#文档)
  - [在 Docker 中启动 n8n](#在-docker-中启动-n8n)
  - [使用隧道启动 n8n](#使用隧道启动-n8n)
  - [使用 PostgreSQL](#使用-postgresql)
  - [使用文件传递敏感数据](#使用文件传递敏感数据)
  - [示例服务器设置](#示例服务器设置)
  - [更新](#更新)
    - [拉取最新（稳定）版本](#拉取最新稳定版本)
    - [拉取特定版本](#拉取特定版本)
    - [拉取下一个（不稳定）版本](#拉取下一个不稳定版本)
    - [使用 Docker Compose 更新](#使用-docker-compose-更新)
  - [设置时区](#设置时区)
  - [构建 Docker 镜像](#构建-docker-镜像)
  - [n8n 的含义及发音](#n8n-的含义及发音)
  - [支持](#支持)
  - [工作机会](#工作机会)
  - [许可证](#许可证)

## 演示

这个 [:tv: 短视频（< 4 分钟）](https://www.youtube.com/watch?v=RpjQTGKm-ok) 介绍了在 n8n 中创建工作流的关键概念。

## 可用集成

n8n 拥有 200 多个不同的节点来自动化工作流。完整列表可以在 [https://n8n.io/integrations](https://n8n.io/integrations) 找到。

## 文档

官方 n8n 文档可以在 [https://docs.n8n.io](https://docs.n8n.io) 找到。

网站上还提供了更多信息和示例工作流，网址为 [https://n8n.io](https://n8n.io)。

## 在 Docker 中启动 n8n

在终端中输入以下内容：

```bash
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```

此命令将下载所需的 n8n 镜像并启动您的容器。
然后，您可以通过打开以下网址访问 n8n：
[http://localhost:5678](http://localhost:5678)

为了在容器重启之间保存您的工作，它还挂载了一个 docker 卷 `n8n_data`。工作流数据保存在用户文件夹（`/home/node/.n8n`）中的 SQLite 数据库中。此文件夹还包含重要数据，如用于保护凭据的 webhook URL 和加密密钥。

如果在启动时找不到这些数据，n8n 会自动创建一个新密钥，任何现有凭据将无法解密。

## 使用隧道启动 n8n

> **警告**：这仅适用于本地开发和测试，不应在生产中使用！

n8n 必须可以从互联网访问，以便使用 webhooks - 这对于从 GitHub 等外部基于网络的服务触发工作流至关重要。为了简化这一过程，n8n 提供了一个特殊的隧道服务，将请求从我们的服务器重定向到您的本地 n8n 实例。您可以在此处查看运行此服务的代码：[https://github.com/n8n-io/localtunnel](https://github.com/n8n-io/localtunnel)

要使用它，只需使用 `--tunnel` 启动 n8n

```bash
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n \
 start --tunnel
```

## 使用 PostgreSQL

默认情况下，n8n 使用 SQLite 保存凭据、过去的执行和工作流。然而，n8n 也支持使用 PostgreSQL。

> **警告**：即使使用不同的数据库，仍然需要持久化 `/home/node/.n8n` 文件夹，其中还包含 n8n 用户数据，包括凭据的加密密钥。

在以下命令中，将占位符（用尖括号表示，例如 `<POSTGRES_USER>`）替换为实际数据：

```bash
docker volume create n8n_data

docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -e DB_TYPE=postgresdb \
 -e DB_POSTGRESDB_DATABASE=<POSTGRES_DATABASE> \
 -e DB_POSTGRESDB_HOST=<POSTGRES_HOST> \
 -e DB_POSTGRESDB_PORT=<POSTGRES_PORT> \
 -e DB_POSTGRESDB_USER=<POSTGRES_USER> \
 -e DB_POSTGRESDB_SCHEMA=<POSTGRES_SCHEMA> \
 -e DB_POSTGRESDB_PASSWORD=<POSTGRES_PASSWORD> \
 -v n8n_data:/home/node/.n8n \
 docker.n8n.io/n8nio/n8n
```

可以在[此处](https://github.com/n8n-io/n8n-hosting/blob/main/docker-compose/withPostgres/README.md)找到使用 docker-compose 的完整工作设置。

## 使用文件传递敏感数据

为了避免通过环境变量传递敏感信息，可以在某些环境变量名称后附加 "_FILE"。n8n 将从具有给定名称的文件中加载数据。这使得可以轻松地从 Docker 和 Kubernetes 机密中加载数据。

以下环境变量支持文件输入：

- DB_POSTGRESDB_DATABASE_FILE
- DB_POSTGRESDB_HOST_FILE
- DB_POSTGRESDB_PASSWORD_FILE
- DB_POSTGRESDB_PORT_FILE
- DB_POSTGRESDB_USER_FILE
- DB_POSTGRESDB_SCHEMA_FILE

## 示例服务器设置

可以在[服务器设置文档](https://docs.n8n.io/hosting/installation/server-setups/)中找到一系列云提供商和场景的示例服务器设置。

## 更新

在升级到最新版本之前，请确保在此处检查是否有可能影响您的重大更改：[重大更改](https://github.com/n8n-io/n8n/blob/master/packages/cli/BREAKING-CHANGES.md)

在您的 Docker Desktop 中，导航到 Images 选项卡并从上下文菜单中选择 Pull 以下载最新的 n8n 镜像。

您还可以使用命令行拉取最新版本或特定版本：

### 拉取最新（稳定）版本

```bash
docker pull docker.n8n.io/n8nio/n8n
```

### 拉取特定版本

```bash
docker pull docker.n8n.io/n8nio/n8n:0.220.1
```

### 拉取下一个（不稳定）版本

```bash
docker pull docker.n8n.io/n8nio/n8n:next
```

停止容器并重新启动：

1. 获取容器 ID：

```bash
docker ps -a
```

2. 使用 ID container_id 停止容器：

```bash
docker stop [container_id]
```

3. 删除容器（这不会删除您的用户数据），使用 ID container_id：

```bash
docker rm [container_id]
```

4. 启动新容器：

```bash
docker run --name=[container_name] [options] -d docker.n8n.io/n8nio/n8n
```

### 使用 Docker Compose 更新

如果您使用 Docker Compose 文件运行 n8n，请按照以下步骤更新 n8n：

```bash
# 拉取最新版本
docker compose pull

# 停止并删除旧版本
docker compose down

# 启动容器
docker compose up -d
```

## 设置时区

要指定 n8n 应使用的时区，可以设置环境变量 `GENERIC_TIMEZONE`。此变量影响的一个示例是 Schedule 节点。

系统的时区可以通过环境变量 `TZ` 单独设置。这控制了某些脚本和命令的输出，例如 `$ date`。

例如，要为两者使用相同的时区：

```bash
docker run -it --rm \
 --name n8n \
 -p 5678:5678 \
 -e GENERIC_TIMEZONE="Europe/Berlin" \
 -e TZ="Europe/Berlin" \
 docker.n8n.io/n8nio/n8n
```

有关配置和环境变量的更多信息，请参阅 [n8n 文档](https://docs.n8n.io/hosting/configuration/environment-variables/)。

## 构建 Docker 镜像

**针对 1.101.0 及更高版本的重要说明：**
构建 n8n Docker 镜像现在需要预编译的 n8n 应用程序。

### 推荐的构建过程：

对于同时处理 n8n 编译和 Docker 镜像创建的最简单方法，请从根目录运行：

```bash
pnpm build:docker
```

### 替代构建器：

如果您使用的构建系统需要单独的构建上下文，请首先编译 n8n 应用程序：

```bash
pnpm run build:deploy
```

然后，确保您的构建器上下文包含此命令生成的 `compiled` 目录。

## n8n 的含义及发音

**简短回答：** 它的意思是 "nodemation"，发音为 n-eight-n。

**详细回答：** 我经常被问到这个问题（比我预期的要多），所以我决定最好在这里回答它。在寻找一个好的项目名称和一个免费的域名时，我很快意识到我能想到的所有好名字都已经被占用了。所以，最后我选择了 nodemation。"node-" 意味着它使用了 Node-View 并且使用了 Node.js，而 "-mation" 则代表 "automation"，这正是该项目旨在帮助实现的。
然而，我不喜欢这个名字太长，我无法想象每次在 CLI 中都写这么长的东西。于是我最终选择了 "n8n"。当然，它并不完美，但 Kubernetes（k8s）也不完美，我没有听到有人抱怨。所以我想这应该没问题。

## 支持

如果您需要更多 n8n 的帮助，可以在 [n8n 社区论坛](https://community.n8n.io) 中寻求支持。这是获取答案的最佳来源，因为 n8n 支持团队和社区成员都可以提供帮助。

## 工作机会

如果您有兴趣为 n8n 工作并塑造项目的未来，请查看我们的[职位发布](https://jobs.ashbyhq.com/n8n)。

## 许可证

您可以在[此处](https://github.com/n8n-io/n8n/blob/master/README.md#license)找到许可证信息。
