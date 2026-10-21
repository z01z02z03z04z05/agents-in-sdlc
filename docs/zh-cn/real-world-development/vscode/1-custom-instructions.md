---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[说明文件][instruction-files]，为代码生成提供指导。

在本练习中，你将：

- 了解如何通过存储库自定义说明和按路径指定的说明文件，为 Copilot 提供项目专属上下文、编码指南和文档标准；
- 在当前说明下生成筛选功能的第一个数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新发送相同提示词，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能基础提交到 `main`，让 Copilot 能在下一课中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的标准。Copilot 的输出具有概率性。本练习的重点是观察更新说明后行为的*趋势*变化，而不是逐字匹配输出。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须始终具备单元测试；
- UI 应采用暗色模式，并具有现代感；
- 代码中应使用 TSDoc 文档注释；
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

- 将 `copilot-instructions.md` 中的内容集中在项目级指导上，例如项目说明、项目结构和全局编码标准；
- 使用 `*.instructions.md` 文件针对文件类型（如单元测试、Astro 组件和数据层）或特定任务提供说明；
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

## 为更改创建分支

我们来遵循最佳实践，为更改创建一个分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型因练习而异；适用时，每一步都会说明如何选择。

要查看自定义说明的影响，先在当前说明文件保持不变时发送提示词。稍后更新说明后，再发送相同提示词并比较差异。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，这样 Copilot 才能应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会涉及辅助函数文件和相关测试。
6. 注意建议的辅助函数是一个类型化函数：第一个参数是 `db` 客户端，返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有明确要求，它有时也会添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新说明后的*一致性*改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来记录存储库编码标准，以改进代码建议。

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

## 重新发送提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意现在建议的文件开头会出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于后续练习要继续使用的实际代码。

## 提交说明并推送分支

说明文件和其他资源一样，使用常规源代码管理方式维护。现在将说明更新和分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础工作更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在应创建拉取请求，并将其关联到待办列表中关于文档标准的议题。可以手动处理，也可以让 Copilot 使用已连接到项目的 GitHub 工具完成：查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已为工作区连接 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这项连接使用的模型上下文协议。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 让 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 提示你允许它代表你执行 GitHub 操作时，先审查命令，再根据需要选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将分支切回 `main`，可以使用以下提示词：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，先审查命令，再根据需要选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并拉取请求！

## 总结和下一步

你了解了 Copilot 如何从说明文件获取项目上下文，并在 VS Code 中使用 Copilot Chat：

- 在现有说明下发送代码生成提示词，并观察 Copilot 生成的内容；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新发送相同提示词，观察建议代码如何采用新标准；
- 将说明更新和辅助函数基础工作提交到 `main`，以便下一课继续使用。

接下来，Copilot 将使用这些说明为整个代码库添加新功能。继续学习[智能体模式][next-lesson]。

## 资源

- [GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
