---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云代理练习前，需要完成准备工作。你将创建 Tailspin Toys 存储库的副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查云代理生成的工作。

## 设置实验室存储库

若要创建用于编写代码的存储库副本，请基于[模板][template-repository]创建一个实例。新实例将包含实验所需的全部文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中，前往 [Tailspin Toys 模板][tailspin-template]。
2. 在实验室存储库页面上选择 **Use this template** 按钮，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后在实验中会用到。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 前往刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，可以了解 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到这个 codespace。目前请将它保留在浏览器选项卡中。

> [!NOTE]
> 本研习营可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都预装了所需的全部组件，可让练习顺利进行。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜你创建了实验室存储库副本！你也已开始创建 codespace，之后会在其中与 Copilot 云代理协作。

## 下一步

接下来，添加云代理将遵循的自定义指令。继续学习[练习 1 - 自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
