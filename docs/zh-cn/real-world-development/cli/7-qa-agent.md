---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建一个 QA 自定义代理，将 issue 要求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到具有明确 QA 流程的自定义代理中。

本课程将介绍如何：

- 探索自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本研习营中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的各类自定义项各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代其他自定义项，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查和必须遵守的限制。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，请检查其配置文件，确认 Copilot 捕获了预期的 QA 工作流和限制。这样可以避免代理不完整或权限过宽，在你只想验证功能时擅自修改功能代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但提供该字段可以清楚地显示代理名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改功能代码前会征求同意，也不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或限制，请让默认代理先修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一筛选对话并选择 QA，以便它使用已有的 issue 和规划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建代理，它可能不会显示在可用代理列表中。代理仍然存在，上面的命令会启用它。

2. 使用以下提示，让 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 代理会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已为工作流添加可复用的专用角色，并审查了它的工作。本课程中，你：

- 探索了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 代理，根据筛选 issue 审查其发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，[整合这些内容创建功能 PR，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
