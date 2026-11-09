---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次修改……工作内容往往不止于此。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来探索如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但智能体模式和云智能体针对的是两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队中的同伴一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表增加，希望用户能够按类别和 publisher 过滤。上一课已经添加了 publisher 辅助函数，接下来将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它当前的功能。

这是一个采用开发者主题的桌游众筹平台，可列出游戏并显示其详情。网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围较小的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一课结束后，当前分支应为 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到用于此过滤功能工作的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅，其中包含网站 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中，选择终端中的 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，此时 Web 服务器正在 codespace 中运行。

### 探索网站

网站运行后，可以探索它的功能。主要功能包括：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏名称、说明、publisher 和类别等信息。

## 与 Copilot 一起探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始干净的对话。不同练习所用的模式和模型不同；相应步骤会在适用时说明。

网站的初始实现已经可用，但还需要添加更多功能。先查看待办事项。在通过模板设置项目时，系统已自动创建一组议题。请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此在说明中列出了该模型，希望由此获得相对一致的代码建议和学习体验。不过，LLM 的行为具有概率性，因此你看到的建议可能与研讨会示例不同。这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示待办议题：

   ```plaintext
   请显示我 GitHub 存储库中的待办事项，并根据哪些事项对用户最有帮助，协助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还会根据它认为对用户最有帮助的程度为事项排序。

## 查看说明文件

让智能体开始生成代码前，先查看将用于向 Copilot 提供工作上下文的说明文件。接下来会使用[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时应采用的方式。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中说明的功能开发总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型对应说明文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用项目中的其他说明文件和文件。路径相对于说明文件所在位置。这样便于重复使用内容、将复杂说明拆分成更易管理的小文件，并提供示例和模板。

## 实现过滤功能

要完成过滤功能，至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 中添加或完善剩余的过滤逻辑。
- 添加或更新过滤行为测试。
- 更新游戏列表页面，加入过滤 UI。

此外，在将更改合并到代码库之前，还需要运行并通过测试。Copilot 智能体模式可以为你完成这些任务，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   请根据待办事项中相关 GitHub 议题的要求，更新网站，支持按 publisher 和类别过滤。上一课已经添加了 publisher 辅助函数；请在完成其余数据层、UI 和测试工作时保留并按需完善该实现。完成前确保所有测试均通过。服务器已经在运行，无需再次启动。
   ```

6. 观察 Copilot 如何先探索项目并查找与目标功能相关的文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂度，可能需要几分钟。此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此不必在工作进行期间选择这些选项。

7. 按 Copilot 的提示选择 **Continue** 以运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 此过程中可能会暂停，也可能会看到一些测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式中已完成的 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 查看生成的代码，检查是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习探索了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。本练习介绍了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习 — 实现分页

随着游戏列表增加，需要添加分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 考虑分页方式：允许用户选择每页大小，还是固定为预设大小。
- 编写提示时，为 Copilot 提供所需指导，帮助它按预期实现功能。
- 可能需要通过多轮交互，请 Copilot 修改并补充上下文。这是与 Copilot 协作时的正常方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
