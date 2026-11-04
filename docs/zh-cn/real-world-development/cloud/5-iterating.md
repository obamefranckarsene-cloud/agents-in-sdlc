---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代改进的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。接下来查看代码更改；必要时向 Copilot 提供反馈，进一步改进其工作。

### 场景

如前面多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的建议，然后再为团队其他成员启用审查。

## 安全性和 GitHub Copilot 云代理

由于 Copilot 云代理异步且无人监督地执行任务，因此必须设置一些安全限制，确保一切安全：

- Copilot 对存储库只有读取权限，对它创建的分支才有写入权限。
- 云代理在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- GitHub Actions 工作流必须先经人工批准才能运行。
- [默认情况下会限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云代理创建的第一个拉取请求（PR），其中包含为代码添加的文档。你将使用 GitHub.com 的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。你可以[自定义或禁用 Copilot 云代理的防火墙][agent-firewall]。

1. 在 github.com 上打开你的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以趁此休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会因结果而异。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自你在练习 2 开始时审查的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作不是单向的。你可以像在 PR 中评论其他团队成员一样，在 PR 评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果并非确定，因此无法规定唯一的提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何开始新会话以进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

如果还不满意，可以继续迭代。满意后，可以将 PR 从草稿状态改为就绪，然后合并到 main 分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完整描述的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到有些工作流正在等待批准。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，你可以迭代使用 Copilot 云代理并请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配该任务时，你还没有更新自定义指令！）。与之前一样，可以在 **Conversation** 选项卡中发表评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。这个 PR 应包括练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完整描述的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现时，尤其关注：
   - 用于切换模式的 UI 组件。
   - 用于持久化用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到有些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查部分排队运行。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且在页面重新加载后仍然保留设置。

请注意，无障碍自定义代理如何帮助 Copilot 根据无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 - 继续委派工作

云代理最适合处理待办列表中的真实任务，让你可以转而处理其他工作。试着养成这种习惯：为存储库创建几个 issue 并分配给 Copilot。可以考虑以下内容：

- 在游戏详情页添加支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理开发环境的全部练习。在这些课程中，你：

- **检查了此存储库自带的自定义指令文件**，以便之后观察它们对云代理输出的影响。
- **将 issue 分配给 Copilot 云代理**，并观察它设置环境、规划和异步执行任务。
- **创建并使用自定义代理**处理无障碍任务，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代改进云代理的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云代理开发环境的全部练习。想继续探索的话，其他开发环境可以补充本次实践内容：

- 🖥️ **[VS Code 开发环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 开发环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接，交付两个经过审查的更改。

在自己的存储库中，还可以尝试以下后续练习：

- 将重构或测试覆盖率 issue 分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
