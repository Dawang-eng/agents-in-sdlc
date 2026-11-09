---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方案，使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚刚为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，确保以正确的方式构建正确的功能。本课是多节课程中的第一课，将带你体验典型的智能体驱动开发流程：从议题开始生成新功能，确认代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本研讨会会略过这些步骤，以便专注于核心概念。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商缩小游戏列表。待办议题描述了此功能，但在开始编写代码前，需要先确认诸如如何组合类别等细节。你将使用 Plan 模式确定这些决策，然后使用 Autopilot 实现范围明确的功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。事实上，这些原则变得更加重要。大多数开发人员的工作流程大致如下：

1. 从描述需求的已登记议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准流程，可确保 AI 生成的代码满足需求，并经过与手动编写的代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，并且无法编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试和迭代，直至完成任务。

先使用 Plan 模式，再审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能之前，返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已经合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选择标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

注意提示现在以 `#7` 或类似编号开头。`#` 可将 GitHub 上的议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

先定义功能或任务的实现方法，再交给 Copilot，有助于明确预期。对于复杂任务，花些时间规划通常是有益的。接下来切换到计划模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 将开始制定计划：先探索项目，再确定最佳实现方法。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答即可，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查完成，现在开始实现。使用 Autopilot 让 Copilot 完成工作。

Autopilot 允许 Copilot 反复处理问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你安装的版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告依赖项缺失或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，在合并之前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败项。

## 探索新功能

代码看起来不错，但能否正常运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 测试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果状态符合议题要求和你的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

已使用不同对话模式构建并审查功能。本课完成了以下操作：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来[使用自定义说明][next-lesson]，深入了解代码生成过程，并确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
