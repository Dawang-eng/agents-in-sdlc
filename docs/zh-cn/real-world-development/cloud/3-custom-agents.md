---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如确保遵循 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含至少设置 `name` 和 `description` 的 YAML frontmatter，后面是用于定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，用来指导 AI 执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它使用的工具。**

自定义智能体有自己的上下文窗口，能够在工作中编排技能甚至其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站；在执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义智能体完成以下练习：

- 了解如何定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期的用户反馈指出，当前暗色主题中文字与背景的对比度不足，导致部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来将使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

已经为你创建了一个无障碍自定义智能体。先查看它的内容，了解它将如何指导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续章节，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以在这里将任务分配给 Copilot 云智能体、监控任务，也可以重定向会话并提供更多指导。现在，先将实现高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `为网站添加高对比度模式`。
6. 将 **Description** 设置为：

    ```plaintext
    我们需要为网站添加高对比度模式。应提供一个由用户控制的高对比度切换开关，并使用浏览器本地存储保留设置。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中高亮显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队专业知识和标准整理成可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来学习如何在[任务控制中心监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
