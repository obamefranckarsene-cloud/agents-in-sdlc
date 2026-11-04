---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支上。现在可以一并审查并创建拉取请求了。星级评分拉取请求（PR）是你手动合并的；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，我们会将功能、指令更新、技能更新和 QA 代理分别放在几个 PR 中。为简化本研习营的流程，这些筛选和质量工作都保留在同一对话和分支中，并通过这个 PR 一并提交。

本课程将介绍如何：

- 了解什么是 Agent Merge，以及它如何自动管理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代劳，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部完成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必要的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。你：

- 了解了什么是 Agent Merge，以及它如何自动管理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索其他实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云代理委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
