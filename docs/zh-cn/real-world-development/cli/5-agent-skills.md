---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已经手动验证代码能否运行，并使用指令文件确保代码遵循标准。测试、lint 和持续集成 (CI) 中的其他环节又该如何处理？

这类任务最适合使用**智能体技能**。技能可以帮助 Copilot 正确执行这些操作。

本课将介绍如何：

- 探索现有的 `quality-checks` 技能。
- 自定义结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易阅读。

## 指令、脚本和资源

智能体技能会打包可复用的任务指令、可执行脚本和支持资源，由智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件包含说明技能用途的 frontmatter（包括名称和描述）、功能概述，以及说明何时应调用技能的指南。文件夹还可以包含供技能使用的脚本和其他资源。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队可共享和复用的存储库资源；也可以放在用户的技能文件夹 `~/.copilot/skills` 中。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有的命令。

## 探索技能

我们来看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述可以帮助 Copilot 判断何时调用技能。
3. 阅读指令，了解它们如何引导 Copilot 执行测试和 lint 流程。
4. 留意该技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

技能既可以由 Copilot CLI 直接调用，也可以通过自然语言调用。让 Copilot 运行该技能的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 阅读最后生成的报告。

## 自定义报告

我们希望报告能说明运行了哪些检查、结果如何，以及工具实际报告了什么。现在更新技能，让它生成这样的报告。

1. 重新打开 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更新完成后，再次查看它的效果。Copilot CLI 可以在不重启对话的情况下重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用之前的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 阅读报告，并与第一次的报告比较。

## 总结与后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

这项更改将与筛选功能一同进入 PR。接下来，[使用 Playwright MCP 服务器][next-lesson]，让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非本研讨会的额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其打包导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
