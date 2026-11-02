---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot 云智能体执行了三项不同任务，用于改进用户体验和添加功能。云智能体可以异步自主工作，但仍然需要监控这些任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 Mission Control 中，可以查看所有有未关闭拉取请求 (PR) 的智能体任务，了解它们执行的操作，还可以引导正在运行的会话。

本练习将介绍如何：

- 使用智能体页面监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

向智能体分配高对比度模式任务后，团队意识到也应该添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此同时实现这两种模式很合理。现在，你将引导智能体的工作，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。你应能看到分配给 Copilot 的任务，包括：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各项步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和所采用的方法而有所不同。

4. 还可以查看右侧显示的 PR 面板，其中列出 PR 和更改的文件，方便进一步监控。

## 引导云智能体

了解当前任务后，现在要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![智能体页面中 Copilot 云智能体任务的截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成

和之前一样，Copilot 会继续处理更新后的任务。收到消息时，它可能正在执行某个步骤；完成该步骤后会将新请求纳入工作。

这同样需要几分钟。等待时，可以回顾并思考已经学习和探索的内容。

## 总结与后续步骤

本练习介绍了 Copilot 智能体页面，这是监控和引导 Copilot 云智能体任务的中心位置。借助 Mission Control，可以跟踪所有正在进行和已完成的任务、审查已执行的工作，也可以调整正在运行的会话。

本练习介绍了如何：

- 使用 Mission Control 和智能体页面监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一练习：自定义智能体][previous-lesson] | [下一练习：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
