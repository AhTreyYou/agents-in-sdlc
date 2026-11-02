---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 中的自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体可以定义在项目的 `.github/agents` 文件夹中，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面再以 Markdown 提示词定义智能体的行为、专长和指令。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；执行任务时，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。为解决无障碍问题，设计团队要求提供可切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。你将使用自定义智能体生成该功能。

本练习将介绍如何：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

项目已包含无障碍自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 分支创建新的分支，用于无障碍功能开发（这样就能将练习 3 中的筛选功能 PR 与本次工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续查看后面的部分，了解其中定义的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 的代码示例。
   - 常见错误和陷阱。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择某个自定义智能体后，该聊天会话便会使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会具体说明。

1. 如果智能体下拉列表中尚未选中 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前智能体；默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会以内嵌差异的形式显示在聊天中，并标出文件路径供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，让你查看智能体修改的文件。

> [!NOTE]
> 该过程可能需要几分钟。Copilot 会实际修改存储库，包括更新现有 Astro 组件、CSS 和相关测试。

下一练习中，你将检查并引导这项正在进行的工作。

## 总结与后续步骤

本练习介绍了 [GitHub Copilot 自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。自定义智能体可以将团队的专业知识和标准整理为可复用的指南，帮助 Copilot 更有效地完成特定工作。

本练习介绍了如何：

- 定义自定义智能体。
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，在更改进行时审查它们，并在同一个会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
