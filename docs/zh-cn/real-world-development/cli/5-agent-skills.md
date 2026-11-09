---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。此前已经手动验证代码可正常运行，并使用说明文件确保代码符合标准。但测试、lint 和持续集成 (CI) 中的其他步骤又该如何处理？

处理这类任务最适合使用**智能体技能**。技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。Markdown 文件包括设置名称和说明的 frontmatter、技能用途概述，以及何时调用技能的指导。文件夹还可以包含供技能使用的脚本和其他资源子文件夹。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

接下来探索 Tailspin Toys 团队创建的 `quality-checks` 技能，该技能用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明可以帮助 Copilot 判断何时调用技能。
3. 阅读说明，留意它如何指导 Copilot 完成测试和 lint 流程。
4. 留意该技能目前还没有 **Results output formatting** 章节。

## 修改前先运行技能

可以直接在 Copilot CLI 中调用技能，也可以使用自然语言调用。现在请 Copilot 运行该技能的三项检查。

1. 返回筛选功能对话，并切换到 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

希望获得更好的报告，清楚说明运行了什么、是否成功，以及工具实际报告的内容。现在更新技能，让它生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下章节：

   ```markdown
   ## 结果输出格式

   完成后，报告每条运行过的命令以及它通过、失败还是受阻。仅在工具报告相应数据时，才包含测试数量、持续时间、错误、警告和其他指标。对任何失败或阻碍项，指出下一步操作。绝不要将跳过或未完成的检查描述为通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在看看修改后的效果。Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告，并与首次运行的报告进行比较。

## 总结与后续步骤

已自定义并使用现有智能体技能。本课完成了以下操作：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的报告格式。
- 重新加载并运行技能。

此更改会随筛选功能一起进入功能 PR。接下来通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
