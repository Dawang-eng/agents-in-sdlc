---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将：

- 探索自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并对照筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

已使用的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。QA 配置文件不会取代它们，也不需要开启另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据对应议题和已确认的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   它应将每项需求标记为通过、失败或受阻，并提供支持性证据。修改实现代码前必须先询问，不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先检查其配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免在只需要验证功能时生成不完整的配置或进行范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段；`name` 是可选字段，但添加后智能体会有清晰的显示名称。
3. 阅读配置文件中的说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请在继续之前要求默认智能体修改。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选功能对话，然后选择 QA，让它使用当前上下文中的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了智能体，它可能不会显示在可用智能体列表中。智能体已经创建，以上命令会启用它。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中的决定审查筛选功能。它可以创建 PR 了吗？
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它提供的报告。

## 总结与后续步骤

已在工作流中添加可复用的专家角色，并审查其工作。本课完成了以下操作：

- 探索自定义智能体如何与说明、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并对照筛选议题审查其发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来[将这些内容汇总到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
