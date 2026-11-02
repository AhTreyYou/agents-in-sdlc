---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相关指导。[指令文件][instruction-files]不仅描述你需要什么代码，还说明代码应如何组织。完成筛选功能后，你将探索 Copilot 使用的指令、添加一项文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一些开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数，也应予以说明）。
- 可复用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过使用指令文件，可以让 Copilot 按照这些实践完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以指定编码约定、库以及注释风格等，让 Copilot 提供更符合需求的建议和代码片段。指令既可以适用于整个存储库，也可以针对特定文件类型。

指令文件有两种：

- `.github/copilot-instructions.md`：存储库中所有请求都会使用的单个指令文件。内容应是适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖某种格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

初始项目已包含一组指令文件。先检查现有内容，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解其中的项目简介和编码指南。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里包括 Astro、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置一个 glob，用来确定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他指令文件和项目文件。这样可以将较大的指令集拆分成可复用的小文件，并引用可供 Copilot 参考的示例。

## 更新指令以符合团队指南

现有文件已经打下了良好基础，但还缺少一项要求。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分最后添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指南

Copilot CLI 会在对话开始时加载存储库指令。保存更改后，退出并恢复筛选功能对话，以便 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。留意新添加的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你探索了 Copilot 如何读取指令文件，并将新标准应用于功能代码。具体来说，你：

- 了解了存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 GitHub Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
