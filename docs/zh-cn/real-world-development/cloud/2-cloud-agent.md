---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成任务后，它会创建一个草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上几乎没有任何文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望在每款游戏的详情页上显示相关游戏，也就是相同类别的其他游戏。暂时不需要精致的实现，只需要能进行用户体验验收测试的版本。此需求目前正在阻塞工作，但优先级低于其他议题。

这类工作很容易被不断推迟，适合交给 Copilot 云智能体异步处理。开发者可以继续处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式与它协作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看其过程。准备好审查建议的解决方案时，Copilot 会在拉取请求中标记你。

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，使用它并不能省去思考。即使看似简单的操作，也可能包含许多复杂环节，因此[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员，分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；如果还没看过，现在可以快速浏览。向 Copilot 云智能体分配任务前，先只读查看存储库中已有的指令文件，之后便能发现它们产生的影响。

在 GitHub 网页界面中打开以下文件；如果已有 Codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定为 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件。后续练习还会再次检查这些效果。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行一些设置脚本，确保所有内容处于可用状态。将任务分配给 Copilot 时也一样，它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行工作。你可以创建[特殊设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 中，让它在开始工作前运行，以便安装所需的开发工具和依赖项。项目已预先配置好该工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点值得留意：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，方便先测试成功与否，而不是等待 Copilot 执行。

## 添加文档

大家都知道文档很重要，但许多项目中的信息已经过时，或者完全缺失。这类技术债务往往得不到处理，降低团队效率，也让维护代码库或帮助新开发者入门变得更困难。Copilot 擅长生成文档，因此很适合把这类工作分配给云智能体。它会在后台生成所需文档，之后可以在练习中审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有出现新的 PR，请等待一两分钟并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 即将完成的任务。
14. 沿 PR 时间线向下滚动，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、理解当前状态、暂停思考并选择合适方案，以及编写代码。

这可能需要几分钟。云智能体的一大优势就是可以异步执行任务，让我们腾出时间处理其他工作。现在就利用这一点：再向 Copilot 云智能体分配一项任务，然后转而为应用编写新功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一项任务的同时，它可以处理另一项任务。为设计团队添加相关游戏也许不需要很长时间，但仍可以利用这段时间完成其他工作。现在将此任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会显示 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式与软件工程师相似，因此你无需持续监控，完成后再进行审查即可。现在可以转而创建和使用自定义智能体。

## 总结与后续步骤

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步工作。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本练习介绍了如何：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用自定义指令；前面的练习已经介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
