---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。智能体完成工作后，会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本课将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保以安全的方式执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上，网站完全缺少文档。首先，他们希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望在每款游戏的详情页中显示相关游戏，也就是属于相同类别的其他游戏。目前不需要精致的实现，只需要一个可供团队进行验收测试的版本。此需求目前是阻碍项，但还有其他优先级更高的议题。

这两项任务很容易被降级到较低优先级，非常适合分配给 Copilot 云智能体。Copilot 云智能体可以异步处理任务，让开发人员先处理其他事项，然后再回来审查 Copilot 的工作并确认结果符合预期。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与人工开发人员协作类似，可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。会话进行期间或完成后，都可以查看 Copilot 会话。提议的解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法，不能省去思考过程。即使看似简单的操作，深入分析后也会发现其复杂性，因此要[谨慎确定 Copilot 云智能体任务的范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段处理、不断学习和试验，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其对 Copilot 的指导作用。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，也可以在此快速浏览。将任务分配给 Copilot 云智能体之前，先以只读方式查看存储库中已有的说明文件，这样之后就能留意它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些说明限定到符合 `**/*.test.ts` 的文件。

在下一节将“代码缺少文档”议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容都来自说明文件。后续练习审查 PR 时还会再次提到这些要求。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式执行任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，让它在智能体开始工作前运行，以便提供所需的开发工具和依赖项。为了让本实验顺利进行并便于学习，此工作流已预先配置。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可以在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为完成这些步骤所需的最低级别。Copilot 会为自己的操作获得独立令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如用于安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在都是 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个重要之处：

- 其中仅定义了一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 留意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流是否成功运行，而不必等 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但大多数项目中的文档要么已经过时，要么完全缺失。这类技术债务往往一直无人处理，降低工作效率，也增加维护代码库或让新开发者加入团队的难度。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档，之后可以在后续练习中审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `代码缺少文档`。
6. 将 **Description** 设置为：

    ```plaintext
    我们组织要求在适当情况下为函数和方法添加 TSDoc 文档注释。遗憾的是，最近的更新没有遵循此标准。我们需要更新现有代码，在文档注释有助于说明行为的地方补充注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办事项列表和即将执行的任务。
14. 向下滚动拉取请求的时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 留意实时会话内容，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及编写代码。

此过程可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你可以处理其他工作。接下来利用这一特点，再分配一项任务给 Copilot 云智能体，同时转而编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组任务。即使为设计团队添加相关游戏区块可能不需要很长时间，利用这段时间也可以完成其他工作。现在将这项任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `在游戏详情页显示相关游戏`。
6. 将 **Description** 设置为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。具体要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏同类别的其他游戏，但不包含该游戏本身。
   - 在游戏详情页中显示 `Related games` 区域，并使用此辅助函数。
   - 处理某款游戏没有相关游戏的情况。
   - 为新辅助函数创建单元测试。
   - 创建 PR 前，确保所有测试均通过。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁边会出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求。Copilot 云智能体的工作方式类似软件工程师，因此无需一直监控它；任务完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步执行任务。Copilot 可用于处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保以安全的方式执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务。接下来开始创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以读取此前课程中介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
