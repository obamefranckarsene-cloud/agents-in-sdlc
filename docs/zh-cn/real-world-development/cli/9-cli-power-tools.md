---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云代理委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多可用于交互的斜杠命令。这些命令可以访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在再来看看其他实用命令。

本课程将介绍如何：

- 使用 `/context` 和 `/compact` 探索 Copilot 管理对话上下文的方式。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云代理的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解一些其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云代理][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理更大或更复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令自行检查或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和可用空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。若要自行选择压缩时机，请使用 `/compact`。切换到无关任务并希望使用全新对话时，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有各自偏好的模型。Copilot CLI 支持查看并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看你所安装版本中的 `/share` 选项。
2. 如果想共享会话，请选择符合需求的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给他人或发布前，请审查导出内容。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库内容或对话内容。

## 可选：扩展并行工作或委派任务

核心研习营已经结束。Copilot CLI 还为大型任务提供了以下命令：

- `/fleet` 可将彼此独立的子任务分配给子代理并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云代理，由它异步工作并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建其他工作树或触发远程工作。尝试前，请开始新的、范围明确的任务，并按照常规工作流审查结果。若要深入了解异步代理工作，请继续学习[云代理研习营][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 斜杠命令可以用于配置 CLI、共享会话以及查看后台信息。本课程中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 管理对话上下文的方式。
- 使用 `/model` 查看可用模型。
- 了解了 `/share` 如何导出会话。
- 探索了用于并行工作、工作树和委派给云代理的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！接下来，[回顾所学内容][next-lesson]并了解继续学习的后续步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
