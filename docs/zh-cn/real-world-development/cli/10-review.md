---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。本次研讨会中，你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令引导智能体，随后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并使用它在真实浏览器中检查筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，检查需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 处理筛选功能 PR。
- 探索管理上下文、模型、共享和可选云委托的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 分支创建：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确呈现未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能、更新指令并将其应用于功能代码、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一个对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前将这些结果与需求和测试覆盖审查结合起来。

添加的测试应弥补实际缺失的覆盖；如果 QA 检查后不需要新增测试，也可能是正确结果。授权合并前，请审查代码和证据；代码更改后，应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常修改可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建的内容、原因和方式，会显著影响生成结果。

## 继续探索

你已完成核心工作流。还可以了解一些 CLI 功能：

- `/review`：请求代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种思路。
- `/fleet`：协调多个彼此独立的子任务并行执行。
- `/worktree`：为单独任务创建隔离的工作树。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的好方法。可以将 Copilot 用于生产代码、个人项目，或者多年来一直想构建的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果想通过终端扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
