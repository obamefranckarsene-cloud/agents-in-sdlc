---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

各类组织或多或少都会面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置在待办列表中的功能请求。GitHub Copilot 云代理可以自主执行更新代码和添加功能等任务。代理完成工作后，会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云代理：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，你会发现项目完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（同类别的其他游戏），让用户可以继续浏览。目前不要求实现精致，只需要足以用于用户体验验收测试。这项工作目前受其他更高优先级的 issue 阻挡。

这两项工作都可能很快被降级为低优先级，非常适合分配给 Copilot 云代理。云代理可以在后台异步完成任务，让开发者先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 介绍 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与人类开发者协作一样，你可以通过多种方式与它合作，包括[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行时或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你！

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法工具，不能替你省去思考过程。看起来简单的操作，深入分析后也可能很复杂，因此[请谨慎界定分配给 Copilot 云代理的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、不断学习、尝试和调整。生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其引导 Copilot 的方式。如果你已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的实际效果；如果还没有，现在可以快速查看一下。将工作分配给 Copilot 云代理前，先只读查看此存储库中的指令文件，稍后就能发现它们的作用。

在 GitHub Web UI 或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些指令限定为 `**/*.test.ts` 文件。

在下一部分将 **Code lacks documentation** issue 分配给云代理时，观察生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自上述指令文件。后续练习审查 PR 时还会再次提及。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保环境就绪。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云代理使用 [GitHub Actions][github-actions] 环境执行工作。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在代理开始工作前运行。这样代理就可以使用所需的开发工具和依赖项。为了让实验顺利进行，该工作流已预先配置，可确保 Copilot 使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单体 Astro 应用迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为*你的步骤*设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果希望在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含名为 `copilot-setup-steps` 的一个作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于在等待 Copilot 运行前测试工作流是否成功。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息都已过时或完全缺失。这类技术债务经常无人处理，会拖慢工作效率，也会增加维护代码库或让新开发者加入团队的难度。Copilot 擅长生成文档，因此非常适合将这类 issue 分配给 Copilot 云代理。它会在后台生成所需文档，之后你可以在后续练习中审查结果。

1. 在新的浏览器选项卡中前往 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解状态、暂停思考并选择合适计划，以及编写代码。

此过程可能需要几分钟。Copilot 云代理的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在利用这一特点：先将另一个任务分配给 Copilot 云代理，然后着手编写代码，为应用添加功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云代理的一项重要优势是可以拆分工作：你专注于一组任务，代理则处理另一组。即使为设计团队添加相关游戏部分不一定要花很长时间，也可以把时间用于其他工作。现在将该任务分配给 Copilot 云代理！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，issue 中的第一条评论旁边会出现 👀，表示 Copilot 已开始处理！

![Copilot 使用 eyes emoji 表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二个请求！Copilot 云代理以类似软件工程师的方式工作，因此你无需主动监控，而可以等任务完成后再审查结果。现在转而创建和使用自定义代理。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot 云代理][copilot-agents]。使用云代理可以将 issue 分配给 Copilot 异步处理。你可以用 Copilot 解决技术债务、创建新功能，或协助将代码迁移到其他框架。

本练习介绍了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云代理正在后台处理任务，现在可以转而创建和使用自定义代理。[Copilot 云代理还支持 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
