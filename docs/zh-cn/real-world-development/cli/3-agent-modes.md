---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流程，帮助我们以正确的方式构建正确的功能。接下来的几节课程将介绍典型的代理驱动开发流程：从 issue 开始创建功能，确保代码有效、行为符合预期，最后成功合并到项目中。

> [!NOTE]
> 在整个功能工作流中，你会继续使用同一对话和分支。通常，不同文件类型的工作会使用不同分支或 PR，但本研习营会简化流程，让你专注于核心概念。

本课程将介绍如何：

- 根据 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程将继续介绍更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理以及创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩充，访问者需要按类别和发行商筛选游戏。待办 issue 描述了这项功能，但在开始编码前，需要先商定类别组合等细节。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者的流程大致如下：

1. 从已提交的 issue 开始，该 issue 描述需要完成的工作。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准方法，可以确保 AI 生成的代码满足要求，并经过与手动编写代码相同的审查流程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能的工作前，请返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动后再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可将 GitHub 上的 issue 或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划能让你在交由 Copilot 执行前，先确定实现功能或执行任务的方法。对于复杂工作，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定合适的实现方式。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何内容与你的决定不一致，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查完毕，现在可以开始实现了！让 Copilot 使用 Autopilot 完成工作。

Autopilot 会让 Copilot 持续迭代，直到它认为问题已经解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要先审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照 issue 以及规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 审查项目检查的输出，并要求 Copilot 解决任何失败项。

## 体验新功能

代码看起来没问题，但能否运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中商定的组合。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课程中，你：

- 根据 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下定义要求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义指令][next-lesson]深入了解如何让代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
