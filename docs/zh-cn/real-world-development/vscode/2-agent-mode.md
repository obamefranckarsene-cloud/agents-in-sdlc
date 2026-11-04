---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：查找所有相关文件、进行修改、运行测试、调试、发现遗漏的文件、再次修改……类似工作还有很多。

Copilot 代理模式正适合处理这类任务。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云代理虽然名称相似，但适用于两种不同体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云代理则像团队成员一样异步工作，通过 issue 和拉取请求与你互动。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、确定相关文件并协调多文件更改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库前审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习中已经添加了发行商辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先了解 Tailspin Toys 网站当前的功能。

此网站是一个围绕开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看游戏详情。整个网站是一个 Astro 应用，会在构建时将页面渲染成静态 HTML。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用少量范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。请按以下步骤在 GitHub Codespace 中运行：

1. 返回上一练习中使用的 codespace。此时应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，选择 Codespace 终端中的 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道；现在 Web 服务器正在其中运行！

### 浏览网站

网站运行后，可以浏览其中的功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace、打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；每一步会在适用时说明。

网站的初始实现已经可用，但我们想增加新功能。先查看待办列表。使用模板设置项目时，系统已经自动创建待办 issue；现在让 GitHub Copilot 列出这些事项。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。Agent 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研习营作者并未偏好某个模型。编写本研习营时使用的是 Claude Sonnet 4.5，因此将其列为示例。我们的目标是让代码建议相对一致，从而提供良好体验。不过，由于 LLM 具有概率性，你收到的建议可能与课程中描述的不同。这是正常现象，也是预期行为。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 审查生成的 issue 列表。

请注意，Copilot 还按它认为对用户最有用的程度为事项排定了优先级。

## 审查指令文件

让代理生成代码前，先查看用于向 Copilot 提供工作上下文的指令文件。接下来将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中包含如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各文件类型专属指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样可以重复使用指导、将较大指令集拆分成更小的文件，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项独立更新：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以替你完成这些工作！现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件示例的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍为模型选择了 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目、找到与该功能相关的文件。你应该会看到它找到数据层辅助函数、UI 和测试，随后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 按钮，因此无需在工作过程中选择它们。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直至完成任务且未检测到错误。

   ![Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、确定相关文件并协调多文件更改。
- 在将代码合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表不断增长，需要启用分页。使用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码提示时可考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示时，提供必要指导，让 Copilot 按照你的设想实现功能。
- 可能需要与 GitHub Copilot 迭代，多次提出更改并补充上下文。这是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
