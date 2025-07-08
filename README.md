![横幅图片](https://user-images.githubusercontent.com/10284570/173569848-c624317f-42b1-45a6-ab09-f0ea3c247648.png)

# n8n - 为技术团队提供安全的工作流自动化

n8n 是一个工作流自动化平台，为技术团队提供了代码的灵活性和无代码的速度。通过 400 多个集成、原生 AI 功能和公平代码许可，n8n 让您在保持对数据和部署的完全控制的同时，构建强大的自动化。

![n8n.io - 截图](https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-screenshot-readme.png)

## 主要功能

- **需要时编写代码**：编写 JavaScript/Python，添加 npm 包，或使用可视化界面
- **AI 原生平台**：基于 LangChain 构建 AI 代理工作流，使用您自己的数据和模型
- **完全控制**：通过我们的公平代码许可自托管或使用我们的[云服务](https://app.n8n.cloud/login)
- **企业就绪**：高级权限、SSO 和隔离部署
- **活跃的社区**：400 多个集成和 900 多个现成的[模板](https://n8n.io/workflows)

## 快速开始

使用 [npx](https://docs.n8n.io/hosting/installation/npm/) 立即尝试 n8n（需要 [Node.js](https://nodejs.org/en/)）：

```
npx n8n
```

或使用 [Docker](https://docs.n8n.io/hosting/installation/docker/) 部署：

```
docker volume create n8n_data
docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

在 http://localhost:5678 访问编辑器

## 资源

- 📚 [文档](https://docs.n8n.io)
- 🔧 [400+ 集成](https://n8n.io/integrations)
- 💡 [示例工作流](https://n8n.io/workflows)
- 🤖 [AI & LangChain 指南](https://docs.n8n.io/langchain/)
- 👥 [社区论坛](https://community.n8n.io)
- 📖 [社区教程](https://community.n8n.io/c/tutorials/28)

## 支持

需要帮助？我们的社区论坛是获取支持和与其他用户联系的地方：[community.n8n.io](https://community.n8n.io)

## 许可

n8n 是 [fair-code](https://faircode.io) 分发的，遵循 [可持续使用许可](https://github.com/n8n-io/n8n/blob/master/LICENSE.md) 和 [n8n 企业许可](https://github.com/n8n-io/n8n/blob/master/LICENSE_EE.md)。

- **源代码可见**：始终可见的源代码
- **可自托管**：可在任何地方部署
- **可扩展**：添加您自己的节点和功能

[企业许可](mailto:license@n8n.io) 可用于额外的功能和支持。

有关许可模型的更多信息，请参阅[文档](https://docs.n8n.io/reference/license/)。

## 贡献

发现错误 🐛 或有功能想法 ✨？查看我们的[贡献指南](https://github.com/n8n-io/n8n/blob/master/CONTRIBUTING.md)以开始。

## 加入团队

想要塑造自动化的未来？查看我们的[职位发布](https://n8n.io/careers)并加入我们的团队！

## n8n 是什么意思？

**简短回答：** 它的意思是"nodemation"，发音为 n-eight-n。

**详细回答：** "我经常被问到这个问题（比我预期的要多），所以我决定最好在这里回答。寻找一个好的项目名称和一个可用的域名时，我很快意识到我能想到的所有好名字都已经被占用了。所以，最后我选择了 nodemation。'node-' 表示它使用 Node-View 并且使用 Node.js，'-mation' 表示 'automation'，这正是该项目旨在帮助实现的。然而，我不喜欢这个名字太长，我无法想象每次在 CLI 中都要写这么长的名字。于是我最终选择了 'n8n'。" - **Jan Oberhauser，创始人兼 CEO，n8n.io**
