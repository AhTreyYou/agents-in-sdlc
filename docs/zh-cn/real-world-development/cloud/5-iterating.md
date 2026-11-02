---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你让 GitHub Copilot 执行了多项任务，以改进用户体验并添加功能：为代码添加文档、构建相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在检查代码更改；如有需要，可以向 Copilot 提供反馈，改进它的工作。

### 场景

如前所述，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍需要审查生成的代码，并遵循正常的 DevOps 流程。现在先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再为团队其他成员启用审查。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此它受到一些安全限制：

- Copilot 对存储库仅有读取权限，只能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先通过 GitHub.com 上的标准 PR 界面，查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会显示 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。等待时可以休息一下，或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能有所不同。检查 TypeScript 约定和文件注释头；这些要求来自练习 2 开始时查看的自定义指令文件。
6. 审查完成且内容无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改内容

与 Copilot 协作并非单向操作。你可以像在 PR 中评论其他团队成员一样标记 Copilot，也可以对代码添加行内评论。Copilot 会读取评论，并启动新的会话来处理请求。由于结果并非确定性生成，因此无法提供适用于所有情况的固定提示。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，像标记其他用户一样标记 **@copilot**，并要求它修改生成的文档。可以使用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 执行更改。它会启动新的会话。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，然后合并到 `main`。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且内容无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中检出该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以通过评论继续迭代。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。在 **Conversation** 选项卡中添加评论即可触发新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 中会显示 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性，例如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成且内容无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中检出该分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认用户偏好能在重新加载页面后保留。

注意，自定义无障碍智能体有助于引导 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进机会，可以像之前一样在评论中标记 **@copilot**，要求它进行更新。

## 可选练习 — 继续委托任务

云智能体最适合处理可以交给它、让你继续做其他工作的真实待办事项。可以在自己的存储库中创建更多议题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列课程中，你：

- **检查了存储库中附带的自定义指令文件**，之后可以在云智能体生成的结果中观察它们的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为 Mission Control**，监控并引导无障碍会话。
- **审查并迭代改进云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体学习路径已完成。其他学习路径可以帮助你进一步了解所学内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接使用 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖任务分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
