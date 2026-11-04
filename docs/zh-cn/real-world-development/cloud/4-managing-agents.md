---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你要求 Copilot 云代理执行了三项独立任务，用于改进用户体验和添加功能。云代理虽然设计为异步、自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个 mission control 中，你可以查看所有具有未关闭拉取请求（PR）的代理任务，探索代理执行的操作，甚至引导正在进行的会话。

本课程将介绍如何：

- 探索代理页面并监控云代理任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给代理后，团队意识到也可以顺便添加浅色模式。由于已经在更新网站样式并添加切换功能，因此实现浅色模式也很合理。现在，你将引导代理工作，确保它同时添加浅色模式和高对比度模式。

## 审查 Copilot 云代理任务

查看目前分配给 Copilot 云代理的任务状态。

1. 前往 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码库的文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 工作状态和采取的方法而异。

4. 同时留意右侧显示的拉取请求（PR）窗格。你可以通过该窗格查看 PR 和更改的文件，以便进一步监控。

## 引导云代理

了解当前活动任务后，让 Copilot 在实现高对比度模式的同时也添加浅色模式切换开关。

1. 选择标题涉及添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似以下消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![云代理任务截图，其中高亮显示 agents 页面中的“Steer active session while copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后将新要求纳入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课程介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot 云代理任务的中心位置。在此 mission control 中，你可以跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向正在进行的任务以调整范围或提供额外指导。

本课程涵盖了以下内容：

- 探索 mission control 和代理页面以监控云代理任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
