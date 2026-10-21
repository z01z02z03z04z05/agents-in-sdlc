---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多个任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队实现可迭代改进的相关游戏功能，并实现包括高对比度和浅色模式切换在内的无障碍访问功能。现在，我们来查看代码更改，并在必要时向 Copilot 提供反馈，改进它的工作。

### 场景

正如我们多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队启用审查功能之前，我们先审查 GitHub Copilot 提供的文档、相关游戏功能和无障碍访问功能实现。

## 安全性与 GitHub Copilot 云端智能体

由于 Copilot 云端智能体会异步执行任务且无需监督，因此系统设置了一些安全限制，以确保工作安全：

- Copilot 仅拥有存储库的读取权限，并且只对用于存放其代码的分支拥有写入权限。
- 云端智能体在 GitHub Actions 中运行，并会创建独立、临时的工作环境。
- 任何 GitHub Actions 工作流都必须先经人工批准才能运行。
- [默认情况下，访问外部资源受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云端智能体创建的第一个拉取请求（PR），它的任务是为代码添加文档。你将使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，你可能会看到提示，指出 GitHub Copilot 被防火墙阻止。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。你也可以[自定义或禁用 Copilot 云端智能体的防火墙][agent-firewall]。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更完善的标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟；你可以暂时休息一下，或回顾此前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增文档注释和其他文档。具体更改会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自你在练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应该会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作处理拉取请求并非单向沟通。你可以像联系团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新会话来处理请求。由于结果并非确定，因此无法规定应该具体询问什么。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释标题；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，提及 **@copilot** 并请求更改生成的文档。可以使用上面的任一建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 的工作。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以持续迭代，直到满意为止。准备好后，可以将 PR 从草稿转换为准备审查状态，并合并到主分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在，我们来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更完善的标题的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 工作流应该会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。
9. **可选：** 你还可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！和之前一样，你可以与 Copilot 云端智能体迭代合作并请求更新。例如，可以调整显示的相关游戏数量，或要求添加文件注释标题和 TSDoc 文档注释（请记住，这个任务是在更新自定义说明之前分配的！）。和之前一样，你可以在 **Conversation** 选项卡中添加评论，让 Copilot 读取并启动新会话。

## 审查无障碍访问功能

最后，我们来审查使用无障碍访问自定义智能体实现的无障碍访问功能。此 PR 应包括练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更完善的标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现时，尤其要留意：
   - 用于切换模式的切换 UI 组件；
   - 使用本地存储保存用户偏好设置的方式；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍访问属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应该会在拉取请求的检查区域中排队运行。如果一切正常，单个 Astro 应用的项目检查就会通过。可能需要几分钟。
10. **可选：** 你可以在 Codespace 中切换到此分支，手动测试无障碍访问功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认它们工作正常，并且在重新加载页面后仍保留设置。

请注意，无障碍访问自定义智能体如何引导 Copilot 遵循无障碍访问最佳实践来实现这些功能。如果发现任何无障碍访问问题或改进空间，可以像处理其他 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云端智能体最适合处理真实待办任务，让你可以专注于其他工作。试着再为存储库创建几个议题并分配给 Copilot，养成使用习惯。可以考虑以下任务：

- 在游戏详情页创建支持者意向表单；
- 为游戏列表页实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端智能体系列练习。在这些课程中，你：

- **查看了存储库附带的自定义说明文件**，了解这些说明对云端智能体输出的影响；
- **将议题分配给 Copilot 云端智能体**，观察它如何设置环境、规划任务并异步执行；
- **创建并使用自定义智能体**，实现高对比度和浅色模式切换；
- **将 Copilot 智能体页面用作任务控制中心**，监控并引导无障碍访问会话；
- **审查并迭代改进云端智能体的拉取请求**，通过提及 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成云端智能体系列练习。如果想继续探索，其他练习系列可以帮助你巩固所学内容：

- 🖥️ **[VS Code 练习][vscode-harness]** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习][cli-harness]** — 在终端中交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端智能体桥接。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云端智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云端智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
