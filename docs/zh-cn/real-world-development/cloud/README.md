---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行 GitHub Copilot。你在 GitHub 上分配工作后，云代理会在后台接手任务，探索存储库、进行更改并创建拉取请求，而你可以继续处理其他事务。

在这些练习中，你将添加云代理要遵循的自定义指令，然后分配一个 GitHub issue 并让它实现相应工作。你还将审查和使用自定义代理来指导其工作方式，在代理仪表板中监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云代理将遵循的自定义指令 |
| [2. 云代理][ex2] | 异步代理 | 将 issue 分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研习营前，请确保具备以下条件：

- [ ] 拥有 GitHub 帐户，并订阅有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的帐户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费计划？经验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研习营使用的代理、MCP、代码审查和 Copilot CLI 功能，因此你可以用它完成全部开发环境的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，所有计划（Copilot Student、Pro、Pro+、Business 和 Enterprise）都可以使用云代理。对于 Copilot Business 或 Enterprise，管理员必须为组织启用该功能。

> [!NOTE]
> 本开发环境的练习不涉及 MCP。若要探索如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 开发环境](../cli/)或 [VS Code 开发环境](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
