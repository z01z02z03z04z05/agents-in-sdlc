---
title: "练习 1 - 自定义说明（云端智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云端智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[说明文件][instruction-files]可提供这些指导，让 Copilot 不仅了解你要它做什么，也了解你希望它如何完成任务。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径指定的说明文件，为 Copilot 提供项目专属上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加一条新的全局标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，此处不会使用“前后对比”提示词。Copilot 云端智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查云端智能体生成的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须始终具备单元测试。
- UI 应采用暗色模式，并具有现代感。
- 代码中应使用 TSDoc 文档注释。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，你可以确保 Copilot 了解相关实践，并据此完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以借此引导 Copilot 提供更符合需求的建议和代码片段。例如，可以指定偏好的编码约定、库，以及希望包含的注释类型。你可以为整个存储库创建说明，也可以针对特定文件类型或任务创建说明。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。此文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都需要了解的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件。你可以为 TypeScript 或 Astro 等语言，或创建 UI 组件、编写单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件只会用于 Copilot Chat 中的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持使用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

关于如何创建说明文件的完整介绍超出了本工作坊的范围。不过，示例项目展示了一种具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的内容集中在项目级指导上，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对文件类型（如单元测试、Astro 组件和数据层）或特定任务提供说明。
- 使用自然语言，让指导清晰易懂，并提供代码示例说明应该和不应该采用的写法。

创建说明文件没有唯一的正确方法，使用 AI 也一样。通过实验，你会逐渐找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。查看此项目中的说明文件时，你会发现其中包含 UI 更新和 [Astro][astro-instructions] 等多种任务的说明。
>
> Copilot 也可以帮你生成说明文件。不同界面提供的方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会在适用时介绍具体操作。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花点时间阅读此存储库附带的说明文件：其中包括一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简述以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 进行的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段，它通过相对于存储库根目录的 glob 模式指定适用文件。此处，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看此项目关于创建单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分为更小、可复用的文件，并为 Copilot 提供参考示例。（其中的路径相对于说明文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求添加代码文档。接下来，你将添加 TSDoc 文档注释和文件注释标题的规则。

## 添加新的存储库标准

接下来要完成本练习中的唯一编辑：添加一条全局项目规则，要求在代码中使用 TSDoc 文档注释和文件级注释标题。云端智能体稍后处理分配给它的议题时会读取此规则。

编辑前，先创建一个分支（云端智能体会读取议题目标分支中的说明；你仍应将修改整洁地提交）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来记录项目编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常靠近第 27 行。查看其中记录的项目编码标准；目前还没有关于代码文档的规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级别（`.github/copilot-instructions.md`）创建全局说明文件，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。存储库级文件适合存放刚添加的文档注释规则等项目级标准。

## 提交、推送并合并说明更新

云端智能体会读取议题目标分支中的说明文件。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，必须先将说明更改合并到 `main`，云端智能体才能使用这些说明。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后，云端智能体处理本练习系列下一课分配的议题时，就会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，也可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和下一步

你了解了 Copilot 如何从说明文件获取项目上下文，并在 `.github/copilot-instructions.md` 中添加了一条新的存储库级标准。后续练习中，你会看到云端智能体生成的拉取请求如何遵循该标准。

接下来，将[第一个议题分配给 Copilot 云端智能体][next-lesson]。

## 资源

- [GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
