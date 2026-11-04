---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证代码能否运行，并使用指令文件确保其遵循规范。但测试、lint 以及持续集成（CI）的其他部分该怎么办？

**代理技能**非常适合这类任务！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课程将介绍如何：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易阅读。

## 指令、脚本和资源

代理技能将可复用的任务指令、可执行脚本和支持资源打包在一起，供代理按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含用于定义技能名称和说明的 frontmatter、技能功能概述，以及说明何时应调用技能的指导。文件夹中还可以包含脚本和其他资源子文件夹，供调用技能时使用。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它直接使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

现在查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 执行测试和 lint 流程。
4. 请注意，技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言调用。现在让 Copilot 运行该技能中的三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

现在改进报告，让它说明运行了什么、是否成功，以及工具实际报告的内容。更新技能，让它生成这样的报告！

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在看看实际效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与第一次的报告比较。

## 总结和后续步骤

你已自定义并使用现有代理技能。本课程中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的格式。
- 重新加载并运行了技能。

此更改会与筛选功能一起提交到功能 PR。接下来，让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
