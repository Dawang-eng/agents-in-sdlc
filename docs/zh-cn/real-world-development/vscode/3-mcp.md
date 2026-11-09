---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

刚刚已使用智能体模式构建过滤功能。在打开拉取请求之前，应该先确认它在浏览器中能够正常工作。与其手动操作应用，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试过滤功能。
- 发布分支，并为过滤功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以访问编辑器之外的资源，从而变得更加强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信，这是智能体与外部工具和服务通信的一种标准方式。

![智能体模式内部工作原理示意图，展示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了一种与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时连接外部工具和服务，从而通过资源获取最新信息，并通过工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问，它充当 AI 智能体与外部工具和服务之间的桥梁。MCP 服务器负责管理 AI 智能体与外部工具（例如现有 API 或 NPM package 等本地工具）之间的通信。每个 MCP 服务器都代表一组不同的工具和资源，供 AI 智能体访问。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用这些 API 创建新存储库、更新现有存储库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化能力。AI 智能体可以使用它浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器可以提供对不同工具和资源的访问。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者，并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予对敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含 `github` 条目，因此上一课中的 Copilot 可以读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 条目旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志表示让 Playwright 在无可见窗口的模式下运行，这对于没有桌面环境的 codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已经安装好。如果之后 Copilot 提示缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择该操作可立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动服务器是可选步骤。如果跳过，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动该服务器，并询问你是否信任它。

## 测试过滤功能

[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可操控的真实浏览器。无需手动操作应用来检查结果，智能体可以打开页面、导航、应用过滤条件并读取结果，然后向你总结观察到的情况。这是在不离开当前对话的情况下，快速确认功能是否符合预期的方式。

Playwright MCP 服务器依赖页面的[无障碍树][playwright-mcp-server]而不是截图来工作。这意味着智能体会基于带标签的结构化元素（按钮、链接、列表项）进行推理，方式与辅助技术类似。因此，一次快速的功能检查，也兼具轻量级的无障碍合理性检查。

服务器已连接且应用正在运行时，请 Copilot 测试刚刚构建的过滤功能：

```text
使用 Playwright MCP 服务器打开浏览器，访问正在运行的应用 http://localhost:4321，并验证新游戏过滤功能：

1. 打开游戏页面，并记录列出的游戏数量。
2. 应用类别过滤条件，确认列表只显示该类别中的游戏。
3. 清除类别过滤条件，再应用 publisher 过滤条件，确认列表只显示该 publisher 的游戏。
4. 同时应用类别和 publisher 过滤条件，确认结果同时符合两个条件。

报告每一步的观察结果，并指出任何行为异常之处。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，依次执行每一步，并反馈发现的内容。将总结与议题中的验收标准进行对照。如果发现异常，可以继续提问，或让 Copilot 在创建拉取请求前修复代码。

> [!NOTE]
> 测试要求应用运行在 `http://localhost:4321`。如果开发服务器已经停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果它提示缺少浏览器，让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 等方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题并创建与该议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   请在存储库中查找与按类别和 publisher 过滤有关的议题。从当前分支创建新的拉取请求，并将其关联到正确的议题。
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你登录 GitHub；按提示完成身份验证。
5. 留意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习完成了以下操作：

- 了解了什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在发布前通过浏览器手动测试过滤功能。
- 发布分支并为过滤功能创建拉取请求。

本练习使用 MCP 服务器来*测试*功能，但 MCP 只是扩展 Copilot 能力的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等特定任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
