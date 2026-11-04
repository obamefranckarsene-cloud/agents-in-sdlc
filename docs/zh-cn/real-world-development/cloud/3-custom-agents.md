---
title: "练习 3 - 自定义代理"
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

你将探索以下内容：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好的限制。近期的用户反馈指出，由于文本和背景颜色的对比度不足，一些用户觉得当前深色主题难以阅读。为解决无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义代理生成相应功能。

## 审查无障碍自定义代理

已经为你创建了无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后审查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览并审查后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配 issue

Mission control 是管理环境中所有代理的中心位置。你可以在此分配任务给 Copilot 云代理、监控任务，甚至重定向并提供额外指导。现在先将实现高对比度模式的任务分配给 Copilot。

1. 前往存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![云代理分配截图，其中高亮显示自定义代理和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域的专用 AI 助手。借助自定义代理，你可以将团队专业知识和标准编写成可复用的代理，引导 Copilot 更有效地完成特定类型的工作。

本课程涵盖了以下内容：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

Copilot 正在后台实现高对比度模式，接下来你可以在 mission control 中[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织启用自定义代理][org-custom-agents]
- [为企业启用自定义代理][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云代理][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
