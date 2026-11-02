---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 中的自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义智能体，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理为可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体可以定义在项目的 `.github/agents` 文件夹中，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面再以 Markdown 提示词定义智能体的行为、专长和指令。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；执行任务时，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍如何：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。为解决无障碍问题，设计团队要求提供可切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。你将使用自定义智能体生成此功能。

## 检查无障碍自定义智能体

项目已包含无障碍自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，并检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续查看后面的部分，了解其中定义的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 的代码示例。
   - 常见错误和陷阱。

## 创建并分配议题

Mission Control 是管理环境中所有智能体的中心位置。你可以在这里为 Copilot 云智能体分配任务、监控任务并引导工作。现在向 Copilot 分配一项实现高对比度模式的任务。

1. 打开自己的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![Copilot 云智能体分配界面，其中突出显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 随即开始在后台处理任务。

## 总结与后续步骤

本练习介绍了 [GitHub Copilot 自定义智能体][custom-agents]，也就是针对特定任务和领域定制的专用 AI 助手。自定义智能体可以将团队的专业知识和标准整理为可复用的指南，帮助 Copilot 更有效地完成特定工作。

本练习介绍了如何：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来前往 Mission Control，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一练习：GitHub Copilot 云智能体][previous-lesson] | [下一练习：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
