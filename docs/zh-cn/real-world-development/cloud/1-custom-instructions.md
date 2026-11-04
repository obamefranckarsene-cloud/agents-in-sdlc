---
title: "练习 1 - 自定义指令（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]是提供指导的一种方式，能让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

本练习将介绍如何：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 开发环境不同，本练习不会演示添加指令前后的提示效果。GitHub Copilot 云代理会异步处理 GitHub issue，因此难以实时并排展示指令的影响。在本开发环境的后续课程中，你审查云代理创建的拉取请求时会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发规范和要求，其中包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加文档。
- 应在每个文件顶部添加一段注释，说明文件的用途。

通过指令文件，你可以确保 Copilot 获取正确的信息，并按这些规范完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。此功能可用于指导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码规范、库，甚至希望在代码中添加的注释类型。可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型创建的文件。你可以为 TypeScript 或 Astro 等语言编写指南，也可以为创建 UI 组件或一组单元测试等任务编写指南。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于如何创建指令文件，可以讨论很多内容，超出了本实验的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保指南清晰，并提供代码应如何编写或不应如何编写的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过不断尝试，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一组完善的指令文件。浏览本项目中的指令文件时，你可能会看到针对多种任务的文件，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同界面提供的功能不同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；相关课程会在适当时介绍。
>
> 需要模板或起点？可以探索 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

请花些时间阅读此存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目的简要说明以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。请特别留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。请注意，其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定适用文件。例如，匹配 `**/*.test.ts` 的任何 TypeScript 测试文件都会应用此指令。
5. 查看此项目的单元测试创建指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小且可重复使用的文件，并为 Copilot 提供可参考的示例。这些路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前尚未要求在代码中添加文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来需要编辑的内容只有一项：添加项目级规则，要求以 TSDoc 文档注释和文件注释头的形式在代码中编写文档。云代理之后处理你分配给它的 issue 时会应用此要求。

编辑前，先创建工作分支（云代理会读取 issue 目标分支中的指令；即使如此，也应规范地提交此更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。它介绍了项目编码标准，但目前尚未规定代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以作为 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 提交、推送并合并指令更新

云代理会读取 issue 目标分支中的指令。下一练习中将 issue 分配给 Copilot 时，Copilot 会从 `main` 分支创建新分支。因此，必须先将指令更改合并到 `main`，云代理才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并合并它。这样，云代理在下一练习中处理分配给它的 issue 时就会读取这些指令。

> [!TIP]
> 如果希望在本研习营中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取本项目的指令文件，并添加了一项新的存储库级标准。之后，在本开发环境的其他练习中审查云代理生成的拉取请求时，会看到它遵循了这项标准。

接下来，[将第一个 issue 分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
