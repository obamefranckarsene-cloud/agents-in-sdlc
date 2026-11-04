---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]是提供指导的一种方式，能让 Copilot 不仅了解你想让它做什么，也了解你希望它如何完成。

本练习将介绍如何：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成第一个筛选数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合某些设定的标准。Copilot 的输出具有概率性。本练习的目标是观察更新指令后行为的*趋势*，而不是要求输出逐字一致。

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

## 为更改创建分支

按照最佳实践，为本次更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 codespace、打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；每一步会在适用时说明。

若要了解自定义指令的影响，先在当前指令文件的基础上发送提示。之后，你会更新指令文件并再次发送相同提示，比较两次结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取你希望提供的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这些内容来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；本练习重点仍是观察更新指令后行为的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。它介绍了项目编码标准，但尚未规定代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以作为 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合放置项目级标准，例如刚添加的文档注释规则。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码，供下一练习继续使用。

## 提交指令并推送分支

指令文件和其他资源一样，使用相同的源代码管理方式管理。现在将指令更新和分支提交到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在需要创建拉取请求，并关联待办列表中的文档标准 issue。可以手动操作，不过 Copilot 已连接到项目 GitHub 工具，不妨让它查找 issue、创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是 Copilot 首次代表你在 GitHub 上操作的练习。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续课程将介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的 issue，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
5. 创建 PR 后，让 Copilot 将其合并，并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. 如果 Copilot 提示允许执行 GitHub 操作和 shell 命令，请审查命令，并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你探索了 Copilot 如何读取指令文件，然后在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察现有指令下 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察建议的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
