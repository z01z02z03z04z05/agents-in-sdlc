---
title: "练习 2 - GitHub Copilot 云端智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置在待办列表中的功能请求。GitHub Copilot 云端智能体专为自动执行更新代码和添加功能等任务而设计。智能体完成工作后，会生成一个草稿 PR，供开发者审查。这样可以将繁琐任务委派出去，加快开发进度，让开发者专注于更重要的工作。

你将使用 Copilot 云端智能体探索以下内容：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 明确限定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望解决一些技术债务。最初负责创建网站的承包商没有妥善编写文档——实际上，你会发现完全缺少文档。第一步是为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望在每个游戏的详情页上显示相关游戏，即同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要能用于 UX 验收测试的内容。虽然这项工作目前阻碍了进度，但还有优先级更高的议题需要先处理。

这两项工作都很容易被降低优先级，很适合分配给 Copilot 云端智能体。它可以异步处理任务，让开发者先处理其他工作，之后再回来审查 Copilot 的结果并确认是否符合预期。

## 认识 GitHub Copilot 云端智能体

[GitHub Copilot 云端智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人工开发者一样在后台执行任务。与人工协作类似，可以通过多种方式分配任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话运行期间或完成后查看 Copilot 会话。准备好审查建议的解决方案时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明的重要性

GitHub Copilot 并没有什么魔法——它不会让你跳过思考过程。即使看似简单的操作，深入分析后也会发现复杂性，因此[应谨慎界定分配给 Copilot 云端智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程伙伴：分阶段工作、学习、试验并逐步调整。生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其对 Copilot 的指导作用。如果已经完成自定义说明的动手练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。在分配任务给 Copilot 云端智能体之前，先只读查看此存储库中的说明文件，以便之后观察它们的影响。

在存储库的 GitHub Web UI 中打开以下文件；如果 codespace 已经运行，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释方面的要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` 前置元数据，它将这些说明的适用范围限定为 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云端智能体时，留意生成的拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释标题。它们来自这些说明文件，稍后审查 PR 时我们还会再次提到。

## 为 Copilot 云端智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切正常。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式执行任务。

云端智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，以便在它开始工作前运行，从而为它提供所需的开发工具和依赖项。为方便实验流程并提供学习机会，我们已提前配置此工作流。它确保 Copilot 可以访问 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低级别。Copilot 将使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器，它可以让你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以测试工作流是否成功运行，而无需等待 Copilot 启动它。

## 添加文档

每个人都明白文档的重要性，但大多数项目要么信息过时，要么完全缺少文档。这类技术债务经常无人处理，降低工作效率，也使代码库更难维护或更难让新开发者加入团队。Copilot 擅长生成文档，因此这是分配给 Copilot 云端智能体的理想任务。它会在后台生成所需文档。下一课会审查它的工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请等待一两分钟，然后刷新浏览器窗口。
12. 几分钟后，应该能看到 Copilot 创建了任务清单。

> [!NOTE]
> Copilot 生成的任务清单可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决议题的最佳方法。

13. 查看任务清单及其将要完成的工作。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云端智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在，我们将利用这一点，再分配一项任务给 Copilot 云端智能体，然后转而为应用编写新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云端智能体的一大优势是可以拆分工作，让你和智能体同时专注于不同任务。为设计团队添加相关游戏部分或许不会花太久，但这段时间也可以用来做其他事情。我们来把它分配给 Copilot 云端智能体！

1. 在 github.com 上返回你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二个请求！Copilot 云端智能体以类似软件工程师的方式工作，因此无需主动监控它；等它完成后再审查即可。现在，我们来创建并使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云端智能体][copilot-agents]。你可以将议题分配给云端智能体，让它异步处理。可以用 Copilot 解决技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了以下内容：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 明确限定议题范围的重要性；
- 将议题分配给 Copilot。

云端智能体正在后台处理任务。接下来，我们将创建并使用自定义智能体。[Copilot 云端智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前模块中介绍的自定义说明。

## 资源

- [关于 Copilot 云端智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云端智能体设置工作流最佳实践][cloud-agent-best-practices]

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
