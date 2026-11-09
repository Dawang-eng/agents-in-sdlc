---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一并审查这些更改并创建拉取请求。你已手动合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理该流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR。为简化研讨会，过滤功能和质量工作流一直保留在同一会话和分支中，所有更改都会放入此 PR。

本课将：

- 了解什么是 Agent Merge，以及它如何自动管理合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 审查完成后授权 Agent Merge，并确认 PR 已合并。

## 场景

在过滤功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动执行剩余的 PR 工作，同时仍由开发人员控制合并授权。

## 介绍 Agent Merge

**Agent Merge** 可以在 GitHub Copilot app 中自动完成合并拉取请求所需的剩余工作。启用后，应用会话会读取拉取请求并解决阻塞项——修复失败的持续集成 (CI) 检查、响应审查意见，以及在需要时执行 rebase——随后在 GitHub 允许时合并拉取请求。它会在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

此前都是由你亲自选择 **Merge pull request**。Agent Merge 可以代为处理，但它编辑代码和合并的能力仍需要明确授权。授予合并权限前，先审查它获准执行的操作及实际工作。

## 使用 Agent Merge 管理 PR

代码已全部创建并审查完成。现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮启动流程。仅在下拉菜单中选择该选项并不会启动流程。

Agent Merge 流程随即启动。它将：

- 使用标题和说明创建拉取请求。
- 如果会话从议题启动，则在说明正文中引用相关议题。
- 对目标分支执行 rebase，或处理潜在的合并冲突。
- 监视 CI 流程，确保所有检查通过。
- 监视 PR 中其他开发人员或 Copilot 代码审查提供的反馈，并进行更新以解决这些意见。
- 可以选择在所有操作成功后自动合并 PR。

让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁边有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    所有必需的检查和审批均通过后再合并 PR。不要绕过存储库保护措施。
    ```

9. 跟进智能体的进度并处理报告的阻塞项。继续之前确认 PR 状态为 **Merged**；仅启用合并权限或看到智能体运行中，并不能证明合并已完成。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护措施或权限不足问题。继续前请先解决这些阻塞项。

## 总结与后续步骤

你已自动执行开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课完成了以下操作：

- 了解了什么是 Agent Merge，以及它如何自动管理合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来[使用现有画布并创建分类画布][next-lesson]，探索与智能体一起检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
