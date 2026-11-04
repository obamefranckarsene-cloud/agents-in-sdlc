---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，就应让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在你已经构建了筛选功能，可以了解 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课程将介绍如何：

- 探索存储库指令和按路径限定的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发规范和要求，其中包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（如有可注入的 `db` 参数，也应予以说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口约定；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过指令文件，你可以确保 Copilot 获取正确的信息，并按这些规范完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。此功能可用于指导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码规范、库，甚至希望在代码中添加的注释类型。可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 不同环境支持的其他指令格式可能不同。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 探索此项目中的自定义指令文件

入门项目中已包含一组指令文件。让我们先了解现有文件，再进行更改并查看其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，留意项目的简要说明和编码指导。此指令适用于 Copilot 在该存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。请注意，其中包含 Astro 文件、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它用于设置指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和现有项目文件的链接。这样可以将较大的指令集拆分成较小且可重复使用的文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队规范

现有文件是不错的起点，但仍缺少一项要求。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 将以下内容添加为最后一条要点：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并逐一阅读已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何读取指令文件，并将新规范应用于功能。具体来说，你：

- 探索了存储库指令和按路径限定的指令文件如何传递给代理。
- 更新指令文件以确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
