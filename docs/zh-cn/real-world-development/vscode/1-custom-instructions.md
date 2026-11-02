---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，应确保它能访问这些内容。你可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导它如何生成代码。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，供 Copilot 在下一练习中使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准，因为 Copilot 的结果并非确定性生成。本练习旨在观察更新指令前后 Copilot 行为的趋势，而不是逐字匹配结果。

## 指令文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码补充文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，可以确保 Copilot 按照这些实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，让 Copilot 提供更符合需求的建议和代码片段。指令既可以适用于整个存储库，也可以针对特定文件类型。

指令文件有两种：

- `.github/copilot-instructions.md`：存储库中所有请求都会使用的单个指令文件。内容应是适用于大多数 Copilot 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件，为 TypeScript、Astro、UI 组件或单元测试等提供指南。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

关于创建指令文件的完整讨论超出了本研讨会的范围。示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 重点用于项目级指南，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供详细指令。
- 使用自然语言，并让指南清晰明了。说明代码应该和不应该采用什么写法。

指令文件没有唯一正确的写法，AI 工具的使用也一样。可以在自己的项目中不断尝试，找到适合的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。浏览本项目文件时，可能会发现有些文件专门用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务。
>
> Copilot 也可以帮你生成指令文件。不同产品的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions** 或 Copilot CLI 中的 `/init`。对应学习路径会在相关步骤中介绍这些功能。
>
> 如需模板或入门示例，可探索 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花点时间阅读存储库中附带的指令文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里包括 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意 `applyTo` 字段；它设置相对于存储库根目录的 glob，用来确定指令适用于哪些文件。这里匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对本项目单元测试的具体指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意它链接到其他指令文件（例如 `unit-tests.instructions.md`）和项目文件。这样可以将较大的指令集拆分成可复用的小文件，并引用 Copilot 可参考的示例。文件中的路径相对于指令文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 创建更改分支

遵循良好实践，为更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会具体说明。

为了观察自定义指令的影响，先在当前指令文件下发送提示。更新指令后，再发送相同提示，比较结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数和测试。
6. 留意建议的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回有类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中应用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它可能会在没有明确要求的情况下添加文档注释。即使如此也没有问题；本练习的重点是观察更新指令后行为是否更一致。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，改进生成的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到大约在第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但尚未规定代码文档，因此生成的辅助函数不会包含文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可放在存储库级的 `.github/copilot-instructions.md` 中，也可放在针对语言、文件类型或任务的 `*.instructions.md` 文件中。存储库级文件适合放置刚添加的文档注释规则等全局标准。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意建议的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习将使用的代码。

## 提交并推送指令更新

指令文件和其他项目资源一样，也需要通过源代码管理进行维护。现在将指令更新和辅助函数基础工作提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改已完成：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新以及 Copilot 为辅助函数基础工作修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也要将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用已连接的 GitHub 工具。请求 Copilot 查找议题、创建关联 PR，然后合并。

> [!NOTE]
> 这是 Copilot 首次代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接（Model Context Protocol）的工作方式。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中 **Agent**。
3. 要求 Copilot 找到与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础工作：

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果系统提示允许 Copilot 代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 将它合并到 `main`，并将本地分支切换回 `main`：

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示允许 Copilot 执行 GitHub 操作和 shell 命令，请先审查命令，再酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你探索了 Copilot 如何读取项目中的指令文件，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，并观察现有指令对生成结果的影响。
- 在 `.github/copilot-instructions.md` 中添加存储库级标准。
- 重新发送相同提示，观察建议的代码如何遵循新标准。
- 将指令更新和发行商辅助函数基础工作提交并合并到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨文件实现新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
