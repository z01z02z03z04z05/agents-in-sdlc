---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。作为开发者，你的工作流程可能包括查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……这样的工作还会持续下去。

这正是 Copilot Agent Mode 的用武之地。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再进行必要的更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 尽管名称相似，智能体模式和云端智能体面向两种不同的体验。智能体模式在 IDE 中执行任务，可以快速反馈和交互。云端智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot Agent Mode 探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能；
- 在合并到代码库前审查 Copilot Agent Mode 生成的更改和测试。

## 场景

游戏列表逐渐变长，你希望用户能够按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先了解 Tailspin Toys 网站当前的功能。

这是一个带有开发者主题的棋盘游戏众筹平台。用户可以列出游戏并查看游戏详情。网站是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则使用范围较小的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器，方便运行网站。按照以下步骤，在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，当前分支应为 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
3. 创建并切换到筛选工作的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，应该会显示类似以下内容的 URL 提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现对话框，提示你打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭它。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost URL 时，它会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的专用隧道，现在你的 Web 服务器就运行在其中。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和描述；
- **游戏详情页**：选择某个游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型因练习而异；适用时，每一步都会说明如何选择。

网站的初始实现已经可以运行，但我们希望添加新功能。先查看待办列表。根据模板创建项目时，系统会自动创建一组议题作为待办列表。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，启用智能体模式。该模式可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并非在推荐某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此将其列入说明。希望这样能让代码建议相对一致，提供良好的体验。不过，由于大语言模型具有概率性，你可能会发现收到的建议与工作坊示例不同。这完全正常，也是预期行为。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示词，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据它认为对用户最有用的程度，为项目排定了优先级。

## 查看说明文件

让智能体开始生成代码前，先查看要用于向 Copilot 提供上下文的说明文件。我们将使用[用户界面（UI）说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，了解如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看文件中关于如何添加功能的总体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍访问原则；
   - 指向各种文件类型对应说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、将复杂说明拆分为更小且易于管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用进行三项独立更新：

- 在数据层（`src/lib/`）中添加或完善剩余的筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页，加入筛选 UI。

此外，在将更改合并到代码库之前，必须运行测试并确保通过。Copilot Agent Mode 可以为你完成这些任务！我们来添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![显示如何选择 UI 说明文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍然选择 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示词，让 Copilot 根据待办列表中相关议题的要求实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与目标功能相关的文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮，因此工作进行时无需选择这些按钮。

7. 按照 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至看到一些测试失败。这没关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且没有发现错误。

   ![Copilot Agent Mode 完整聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 查看生成的代码，检查是否存在潜在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，探索新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习中，我们了解了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你学会了如何：

- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能；
- 使用 Copilot Agent Mode 探索项目、识别相关文件并协调更改；
- 在合并到代码库前审查 Copilot Agent Mode 生成的更改和测试。

现在，让我们[使用 Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习 — 实现分页

游戏列表不断增长后，就需要支持分页。运用本练习中学到的技能，让 Copilot 更新网站并实现分页。提示词可以考虑以下事项：

- 遵循现有最佳实践，包括使用已有说明文件；
- 考虑如何实现分页，以及是否允许用户选择每页大小，还是使用固定值；
- 编写提示词时提供 Copilot 所需的指导，确保实现符合预期；
- 可能需要与 GitHub Copilot 反复沟通，提出更改请求并提供上下文。这是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
