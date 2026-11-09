---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

已在一个连续的 Tailspin Toys 工作流中使用 GitHub Copilot CLI。本系列练习完成了以下操作：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定实现方案，在 Autopilot 模式中构建功能，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并使用真实浏览器探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能运行结果和浏览器证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 已交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并将其应用到功能开发、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为了简化研讨会，这些工作都合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会将这些结果与需求和测试覆盖率审查结合起来，再交由 PR 审查。

新增测试应解决实际缺口；如果 QA 检查后无需添加测试，也可能是合理结果。合并前请审查代码和证据；如果更改影响了已有证据，请重新收集。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义智能体定义专家角色，MCP 服务器则连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 在开始构建前思考实现方案，使用 **Interactive** 关注具体更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可以选择响应更快的模型，复杂任务则可以选择能力更强的模型。

上下文与基础设施同样重要。清楚说明希望构建*什么*、*为什么*构建以及*如何*实现，会显著影响输出。

## 继续探索

已完成核心工作流。Copilot CLI 还有一些值得了解的功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离处理单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式。可以用它处理生产代码、兴趣项目，或多年来一直想构建却尚未动手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
