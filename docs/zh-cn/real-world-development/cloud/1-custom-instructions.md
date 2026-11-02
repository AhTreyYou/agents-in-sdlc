---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，应确保它能访问这些内容。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 了解你需要什么，以及希望它如何完成任务。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较更新指令前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示差异。后续练习中审查云智能体创建的拉取请求时，可以看到指令文件带来的影响。

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

- `.github/copilot-instructions.md`：存储库中所有请求都会使用的单个指令文件。内容应是适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
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

## 添加存储库级标准

接下来只需进行一项编辑：添加一条项目级规则，要求代码中使用 TSDoc 文档注释和文件注释头。后续练习中，云智能体会在处理你分配的议题时使用该规则。

编辑之前先创建分支。云智能体会读取议题目标分支中的指令，因此你仍应单独提交这项更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中，将从 `main` 分支创建云智能体任务，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该 PR。这样，云智能体在下一练习中处理议题时就能读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上操作，也可以跳过分支步骤并将更改直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何读取项目中的指令文件，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。后续练习中，可以在云智能体生成的拉取请求中看到该标准的实际效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 的自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
