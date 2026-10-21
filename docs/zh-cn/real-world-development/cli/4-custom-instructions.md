---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述你需要什么代码，也说明代码应该如何组织。现在你已经实现了筛选功能，接下来将查看 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将学习如何：

- 了解存储库说明和按路径指定的说明文件如何传递给智能体；
- 更新说明文件，确保遵循编码标准；
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是重复代码内容；
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应记录；
- 可复用 Astro 组件应记录其 `Props` 约定；相关代码更改时，注释也应保持更新；
- 应保留现有格式和 lint 指导。

通过使用说明文件，你可以确保 Copilot 了解相关实践，并据此完成任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以借此引导 Copilot 提供更符合需求的建议和代码片段。例如，可以指定偏好的编码约定、库，以及希望包含的注释类型。你可以为整个存储库创建说明，也可以针对特定文件类型或任务创建说明。

说明文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。此文件应包含大多数请求都需要了解的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式和支持情况因环境而异。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

入门项目已经包含了一组说明文件。先看看现有文件，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些说明适用于你在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro 文件、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置相对于存储库根目录的 glob，用于确定这些说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为更小、可复用的文件，并为 Copilot 提供参考示例。

## 更新说明文件以符合团队指导

现有文件是一个不错的起点，但仍有不足。我们来修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存说明后，退出并恢复筛选对话。这样 Copilot 会在保留功能上下文的同时，使用新指导。

1. 如果筛选对话仍在运行，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，阅读已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明代码内容。

## 总结和下一步

你了解了 Copilot 如何从说明文件获取上下文，并将新标准应用于功能实现。具体来说，你：

- 了解了存储库说明和按路径指定的说明文件如何传递给智能体；
- 更新了说明文件，确保遵循编码标准；
- 了解了说明文件对代码的影响。

接下来，将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
