---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到了预置的筛选 issue。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 入手，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令引导代理、自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享和可选云代理委派的斜杠命令。

## 你交付的内容

本研习营包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化研习营流程，这些工作都合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 综合这些结果，在 PR 创建前审查了需求和测试覆盖率。

新增测试应解决实际缺口；如果 QA 检查后认为不需要新增测试，也可能是合理的。授权合并前，请审查代码和证据；更改后，重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研习营中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。这些自定义项可在不同对话中重复使用，也可以根据团队需求调整。指令定义标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 处理需要密切互动的专注更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 继续探索

核心工作流已经介绍完毕。还可以了解一些其他 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot 云代理。

## 后续步骤

提升工具使用能力的最佳方法就是继续使用它！可将其用于生产代码、个人项目，或多年来一直想构建却没时间动手的小应用。与团队分享经验，也向同事学习。最后，别忘了探索文档。

如果想通过终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他开发环境，可以探索 [VS Code 研习营][vscode]、[GitHub Copilot 应用研习营][app]或 [Copilot 云代理研习营][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
