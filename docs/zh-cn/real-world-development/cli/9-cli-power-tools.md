---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派任务的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。使用这些命令可以访问高级功能、查看后台信息并配置其他选项。此前已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索并行工作、工作树和委派任务给云智能体等可选命令。

## 场景

核心 CLI 工作流已完成。接下来探索一些其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理更大或更复杂的任务时，可能会接近模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令自行检查或压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 使用情况，以及系统说明、工具、消息和空闲空间的上下文分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果当前对话很短，变化可能不明显。

> [!NOTE]
> 当上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时间时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 可以列出模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享的链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享本次会话，请选择适合的目标，例如 `/share file`，导出到本地 Markdown 文件。
3. 发送给他人或发布之前，先审查导出内容。会话导出内容可能包含提示、回复和项目详情。

是否发布链接或 gist 完全由你决定。不要发布团队无意共享的存储库或对话内容。

## 可选：扩大并行处理范围或委派任务

核心研讨会已经结束。Copilot CLI 还为大型任务提供以下命令：

- `/fleet` 可以将独立子任务分配给子智能体并行运行。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体，由它异步工作并可能创建拉取请求。

这些命令不是必需的，因为它们可能创建额外的工作树或远程工作。试用这些命令前，先启动新的、范围明确的任务，并通过正常工作流审查结果。若要深入了解异步智能体工作，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结与后续步骤

Copilot CLI 中的斜杠命令可用于配置 CLI、共享会话，以及了解后台正在发生的事情。本课完成了以下操作：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索并行工作、工作树和委派任务给云智能体等可选命令。

Copilot CLI 还有其他斜杠命令和更多功能可以探索。接下来[回顾所学内容][next-lesson]，并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
