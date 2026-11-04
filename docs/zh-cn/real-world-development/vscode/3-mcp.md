---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常工作。你将连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器为你测试功能，而不必亲自逐个点击应用；然后发布分支并创建 PR。

本练习将介绍如何：

- 了解什么是模型上下文协议（MCP），以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式能够访问编辑器之外的资源时，它的能力会大幅增强。Copilot 使用模型上下文协议（MCP）实现这一点：这是一种让代理与外部工具和服务通信的标准方式。

![示意图展示代理模式内部如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时通信，从而访问最新信息（使用资源）并代表你执行操作（使用工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，并负责管理 AI 代理与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 代理可以使用这些 API 创建存储库、更新现有存储库，以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 代理可以使用它访问网页、填写表单和选择按钮。

还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 出于安全考虑，请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者并考虑安全影响。只使用你信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 项，因此上一练习中 Copilot 才能读取待办 issue。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容；VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控的浏览器，正好适合用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 项，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器；codespace 中没有桌面环境，因此必须使用该标记。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择该操作立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问你是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要该服务器工具时启动它，并询问你是否信任服务器。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可操控的真实浏览器。代理可以打开页面、导航、应用筛选条件，并将结果报告给你，而无需你亲自点击应用。它还会总结观察结果。这是无需离开对话即可快速确认功能行为是否符合预期的方法。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，代理可以像辅助技术一样读取结构化且带标签的元素（按钮、链接和列表项），快速进行的功能检查也能兼顾基本的无障碍检查。

服务器已连接且应用正在运行后，请让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。请根据 issue 中的验收标准审查其摘要。如果发现异常，请提出后续问题，或让它在创建拉取请求前修复代码。

> [!NOTE]
> 测试期间，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示更改内容的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，包括使用 github.com 和 GitHub 命令行界面（CLI）。既然你已经在使用 GitHub Copilot，就让它来创建 PR 吧！它可以找到相关 issue，并创建与 issue 关联的 PR。

1. 打开 Copilot Chat 面板，并选择 **New Chat** 开始新会话。
2. 确认代理下拉列表中选中了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证；按提示完成身份验证。
5. 留意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接以审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了什么是模型上下文协议（MCP），以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在发布前通过浏览器手动测试了筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器**测试**了一项功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
