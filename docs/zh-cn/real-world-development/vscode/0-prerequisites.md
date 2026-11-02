---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要做好准备。你将创建自己的 Tailspin Toys 存储库副本，启动用于工作的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 可以正常使用。

## 设置实验存储库

通过[模板][template-repository]创建存储库副本，以便存放你编写的代码。新存储库包含实验所需的全部文件。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面选择 **Use this template**，然后选择 **Create a new repository**，创建自己的存储库副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你是在 GitHub 或 Microsoft 主办的活动中参加研讨会，请按照导师提供的说明操作。否则，可以在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建的存储库路径 (**organization-or-user-name/repository-name**)，后续实验会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 通过模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个研讨会都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成实验。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它是功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间可以探索 GitHub Copilot 的其他功能，接下来就会开始介绍它们。

> [!CAUTION]
> 后续练习还会回到此 Codespace。现在请先让它保持打开，并在浏览器标签页中留着。

> [!NOTE]
> 本研讨会可在 Codespace 或本地 [dev container][dev-containers] 中运行。两者都包含顺利完成实验所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 在 VS Code 中使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用 GitHub Codespace，通常已经安装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。此操作应会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 账户。按照提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜，你已创建实验存储库的副本，并开始创建 Codespace。之后将在其中编写代码。

## 下一步

现在让 Copilot 开始工作。继续学习[练习 1：自定义指令][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [通过模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
