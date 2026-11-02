---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发者需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件，再继续修改……工作内容往往不断重复。

Copilot 智能体模式可以帮助处理这些工作。

在 IDE 中，Copilot 智能体模式可以更自主地工作。它会先探索现有项目结构、进行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，适合快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、找到相关文件并协调多文件更改。
- 使用 Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表增加，用户需要按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

开始更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

此网站是一个以开发者为主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能使用精简的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，它会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前托管 Web 服务器的 Codespace 私有隧道。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会具体说明。

网站的初始版本可以正常使用，但还需要添加更多功能。先查看待办事项。通过模板创建项目时，系统已自动创建一组议题。让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此在步骤中列出该模型，希望由此获得相对一致的体验。但由于 LLM 具有概率性，你看到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用其他 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 GitHub 议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 也会根据它认为对用户最有用的程度为议题排序。

## 检查指令文件

让智能体生成代码前，先检查将为 Copilot 提供上下文的指令文件。项目中的 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)提供了网站功能开发指南。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意文件中的整体指导，包括：
   - 项目架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件本身，因此可以复用指南、将复杂指令拆分为较小部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更改：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还必须运行测试并确保通过。Copilot 智能体模式可以完成这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认在 Chat 视图的智能体下拉列表中选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。它应该会找到数据层辅助函数、UI 和测试，随后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库大小，这可能需要几分钟。过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示这两个按钮，因此无需在工作进行期间选择它们。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 整个过程中可能会有暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间不断迭代，直至任务完成且未发现错误。

   ![Copilot 智能体模式的完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。你了解到：

- GitHub Copilot 智能体模式可以跨 UI 和数据层实现新功能。
- Copilot 智能体模式可以探索项目、找到相关文件并协调更改。
- 在将代码合并到代码库之前，应审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习——实现分页

游戏列表增加后，需要实现分页。使用本练习中学到的技能，提示 Copilot 更新网站并实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 决定分页的工作方式，例如让用户选择每页项目数，或使用固定值。
- 编写提示时提供足够指导，确保 Copilot 按照你的设想实现功能。
- 你可能需要与 GitHub Copilot 迭代，要求它修改结果并提供上下文。这是与 Copilot 协作时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
