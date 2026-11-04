---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可创建专门针对开发工作流中特定任务或领域的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以提供专注的指令、最佳实践、编码模式和领域知识，指导 Copilot 更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的代理，例如确保符合 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是用于定义代理行为、专长和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行者，**技能**是工具。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的“正确”方式。与 AI 领域的其他工作一样，请进行测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好的限制。近期的用户反馈指出，由于文本和背景颜色的对比度不足，一些用户觉得当前深色主题难以阅读。为解决无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义代理生成相应功能。

本练习将介绍如何：

- 审查现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 审查无障碍自定义代理

已经为你创建了无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 创建用于无障碍工作的新分支（这样，筛选 PR 将与练习 3 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，聊天会话将遵循该代理的指令和工具设置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace、打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；每一步会在适用时说明。

1. 如果 Chat 视图的代理下拉列表尚未选中 **Agent**，请选择它。

   ![Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（它会显示当前活动的代理，默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑生效，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库，可能会修改 Astro 组件、CSS 和相关测试等现有文件。

下一练习将介绍如何审查并引导这项正在进行的工作。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域的专用 AI 助手。借助自定义代理，你可以将团队专业知识和标准编写成可复用的代理，引导 Copilot 更有效地完成特定类型的工作。

本课程涵盖了以下内容：

- 自定义代理的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，你将[监控并引导代理工作][next-lesson]，实时审查更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
