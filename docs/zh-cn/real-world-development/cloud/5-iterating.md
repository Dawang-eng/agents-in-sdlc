---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本系列练习中，你与 GitHub Copilot 一起完成了多项旨在改善用户体验和添加功能的任务。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，并实现高对比度和浅色模式切换等无障碍功能。接下来检查代码更改；必要时向 Copilot 提供反馈，进一步改进其工作。

### 场景

正如前面多次强调的，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员启用审查之前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改。

## GitHub Copilot 云智能体安全

Copilot 云智能体会在无人监督的情况下异步执行任务，因此需要采取一些安全限制：

- Copilot 只能读取存储库，并且**只能**写入用于处理任务的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含为代码添加的文档。此处将使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问许多外部资源，包括外部 MCP 服务器。也可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。等待时可以休息一下，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    检查更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应在拉取请求检查区域中排队。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向操作。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果具有非确定性，无法提供适用于所有情况的固定提示。可以要求 Copilot：

- 在每个代码文件开头添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论并像提及其他用户一样标记 **@copilot**，要求它更改生成的文档。可以使用上述建议，也可以提出其他文档改进建议。
2. 选择 **View Session**，观察 Copilot 如何执行操作。Copilot 会启动新会话以完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为可审查状态，然后合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。标题也可能更具体。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 工作流应在拉取请求检查区域中排队。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能。与之前一样，可以通过迭代向 Copilot 云智能体请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住：分配此任务时，尚未更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。标题也可能更具体。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应在拉取请求检查区域中排队。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式开关，确认功能符合预期且在重新加载页面后仍能保留设置。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理实际待办任务，让你可以转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，培养这种工作习惯。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

已完成云智能体练习系列。在这些课程中，你：

- **检查了存储库附带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换功能。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

已完成云智能体练习系列。若要继续探索，可以学习其他练习系列：

- 🖥️ **[VS Code 练习系列](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习系列](../../cli/)** — 在终端中交付两项经过审查的更改，探索 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同的技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
