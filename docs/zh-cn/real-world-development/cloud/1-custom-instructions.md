---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解你要完成什么，也知道应该如何完成。

本练习将：

- 探索如何通过存储库自定义说明和按路径限定范围的说明文件，为 Copilot 提供项目上下文、编码准则和文档标准。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习系列不同，本练习不会比较提示前后的结果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示差异。后续审查云智能体创建的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以指导 Copilot 生成更符合需求的建议和代码片段。你可以指定编码约定、首选库，甚至希望添加的注释类型。说明既可以适用于整个存储库，也可以仅针对特定文件类型或任务。

说明文件有两种类型：

- `.github/copilot-instructions.md`：单个说明文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件。你可以用它们为 TypeScript 或 Astro 等语言提供指导，也可以针对创建 UI 组件或单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准中的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

本研讨会无法全面介绍如何创建说明文件，但示例项目展示了一种有代表性的做法。总体而言：

- `copilot-instructions.md` 应聚焦项目级指导，例如项目概述、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码示例说明应该和不应该采用的写法。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过实践，找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护一套完善的说明文件。浏览本项目中的文件时，会看到针对多种任务的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关处介绍这些入口。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：其中有一个核心文件 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各类交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定了相对于存储库根目录的 glob 模式。此处，所有符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 留意专门针对本项目单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并向 Copilot 提供可参考的示例。这里的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一条项目级规则，要求以 TSDoc 文档注释和文件级注释头的形式在代码中编写文档。云智能体稍后处理本系列练习中分配的议题时会读取这条规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的说明；不过，此处仍会规范地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加项目编码标准，要求在代码中包含 TSDoc 文档注释和文件级注释头。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节（应该接近第 27 行）。留意其中记录的项目编码标准；目前还没有关于代码文档的规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件以提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。你刚刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一课将从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，云智能体在下一课处理分配给它的议题时就能读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了让练习更接近真实项目中的工作方式。

## 总结与后续步骤

已探索 Copilot 如何读取本项目说明文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。在本系列练习的后续部分审查云智能体创建的拉取请求时，可以看到这条标准的实际效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
