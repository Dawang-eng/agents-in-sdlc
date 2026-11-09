---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所强调的，编写代码不只是编写代码。还需要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。这正是 MCP 服务器的用途。MCP 服务器可以让 Copilot 使用 CLI 内置功能以外的工具和服务。

本课将：

- 了解什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，仍需要实际与 UI 交互。希望 Copilot 能像用户一样使用网站，进一步自动化更改过程，并提高对更新符合预期的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了一种与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，从而获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问，它充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表一组不同的工具和资源，供 AI 智能体访问。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 探索功能

之前已手动确认功能符合预期。现在使用 Copilot 中可用的 Playwright MCP 服务器执行同样的测试。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照议题和计划检查筛选功能。告诉我哪些功能正常、哪些不正常，但不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常无需明确告诉 Copilot 使用 MCP 服务器，它往往会自行判断。但既然知道应该使用哪个服务器，指出来可以帮助它更稳定地完成任务，同时节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，以确认功能是否正常。
3. 阅读报告并确认所有行为符合预期。

Copilot 会启动服务器，通过 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结与后续步骤

恭喜！已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器探索功能。本课完成了以下操作：

- 了解了什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请智能体驱动浏览器并探索筛选功能。

接下来[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合起来，打造专用角色。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
