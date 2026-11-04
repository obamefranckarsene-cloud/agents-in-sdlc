---
title: "课程 1 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于代理驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道都可以开箱即用。它面向这样的工作流：你同时指挥多个代理，每个代理都在独立工作区中运行，而不是亲自完成所有工作；还可以自动执行重复任务。Node.js 已安装、项目副本也已准备就绪，现在可以安装应用并连接该存储库。

本课程将介绍如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板为你创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正采用 AI 代理来处理日益增长的待办列表。Copilot app 为你提供一个统一位置，用于指派 issue、运行代理、审查更改并合并拉取请求。本课程将帮助你完成安装和连接，并熟悉如何开始讨论项目。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

要使用 GitHub Copilot app，第一步自然是安装它。应用支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证，并将 Tailspin Toys 存储库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 首页][download-app]。
2. 下载适用于当前平台的应用，并按照首页提供的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按提示完成身份验证。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程提供存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在完成引导后再添加项目。
6. 按提示选择主题，然后完成引导流程。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，则无需再次添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。使用先决条件课程中记下的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。此处所用布局会将 `tailspin-toys` 等存储库文件夹归在 **Sessions** 下，每个存储库下则显示对应会话。

## 熟悉工作区

项目连接后，花些时间了解工作区。应用在侧边栏中分成几个区域：

- **New**：顾名思义，可在此启动与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示拉取请求。在这里，无需离开应用即可浏览和筛选拉取请求、查看 CI 状态并审查拉取请求。
- **Issues**：以同样的方式显示 issue。在这里可以浏览和筛选 issue，并从 issue 启动会话。
- **Automations**：按计划或按需运行的已保存代理任务。适合管理待办列表、定期项目维护或其他想交给代理的繁琐工作。总结部分会将其作为后续学习方向，而不是另一项研习营练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件，为 Copilot app 添加功能。你将用它配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量对话，无需单独创建分支或工作区。开始首次聊天后，此区域就会显示。如果未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按存储库文件夹分组显示代理会话。选择存储库名称（例如 `tailspin-toys`）可打开存储库视图，选择其下的会话可返回对应对话。每个会话都在独立的隔离工作区中运行，因此可以同时运行多个会话而不会发生更改冲突。添加星级评分时，你将启动第一个会话。

完成研习营时，你会逐步了解工作区！

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作，或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找预置的待办列表

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在探索创建模板时预先添加的待办列表。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下 issue，不要假设它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个 issue 并阅读详情。每个 issue 也可以作为启动代理会话的入口。完成一项快速的首次更改后，你将从筛选 issue 开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，仅显示已添加到 Copilot app 的存储库中的事项。若要查看其他存储库中的工作项，请将这些存储库添加到应用。

## 尝试快速聊天

熟悉应用的一种好方法是用它来了解*应用本身*，而**快速聊天**正适合这种场景。快速聊天无需创建分支或工作树，即可提问或头脑风暴，非常适合无需会话的一次性问题。

1. 在侧边栏中，选择 **Chats** 旁的 **+** 打开新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁的 **+** 开始第一次聊天；之后就会显示 **Chats**。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。你会了解到，每个会话都在独立的 git 工作树中运行，因此可以并行运行多个代理而不会发生更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并探索工作区。你学习了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区并在 **Issues** 中查找预置的待办列表。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个代理会话][next-lesson]，让它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用代理会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
