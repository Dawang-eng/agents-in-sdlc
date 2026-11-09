---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体分别处理了三项任务，目标都是改善用户体验并添加功能。云智能体虽然可以异步、自主地工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有包含未关闭拉取请求的智能体任务，了解智能体执行的操作，还可以引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并实现浅色模式。接下来将引导智能体，让它在高对比度模式之外也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含已分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，包括：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 具体显示哪些步骤会因 Copilot 的工作状态和执行方式而异。

4. 还可以查看右侧显示的拉取请求窗格，其中包含拉取请求和已更改文件，便于进一步监控。

## 引导云智能体

现在已经查看正在运行的任务，接下来要求 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择描述添加高对比度模式的会话。会话标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。此时会看到类似下面的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。浅色模式也应提供一个切换开关，让用户选择所需的显示模式。
    ```

    ![智能体页面中的云智能体任务截图，其中高亮显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将其纳入工作流程。

## 等待 Copilot 完成

与之前一样，Copilot 会继续处理更新后的任务。在完成收到消息时正在执行的步骤后，它会将新请求加入工作流程。

此过程可能需要几分钟。等待时，可以回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在任务控制中心，可以跟踪所有正在进行和已完成的任务、审查智能体执行的工作，也可以重定向正在运行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来学习如何[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
