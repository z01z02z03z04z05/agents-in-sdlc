---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监控和管理智能体][previous-lesson] |
| :-- |

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多个任务，以改善用户体验。你使用智能体模式在客户端和服务器端添加筛选功能，通过 Playwright MCP 服务器在浏览器中手动测试，再使用自定义智能体实现高对比度和浅色模式切换等无障碍功能，并在会话进行中引导智能体扩展工作。现在应发布这些本地工作，并按照团队的方式进行审查。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。Copilot 生成的任何成果仍然需要完整的审查流程。因此，我们将从 codespace 推送无障碍功能更改、创建拉取请求，并在邀请团队其他成员参与前逐项审查差异。

## 发布无障碍访问功能

你在[练习 4][exercise-4]和[练习 5][exercise-5]中使用无障碍访问自定义智能体实现的高对比度和浅色模式切换，已作为提交保存在 codespace 中。现在将其推送到分支并创建拉取请求，供团队其他成员审查。

1. 返回 codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍访问更改已提交。如果练习 5 中还有未提交的更改，现在暂存并提交，使用描述性消息，例如 `Add high-contrast and light-mode toggles`。
4. 选择 **Publish Branch** 发布分支（或选择 **...** 菜单 → **Push**）。
5. VS Code 会提示你在 github.com 上打开新分支。接受提示，或手动转到存储库，并在分支横幅中选择 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并用简短描述概括完成的工作及其原因。
7. 选择 **Create pull request**。
8. 打开 PR 后，选择 **Files changed** 选项卡，端到端审查工作。重点关注：
   - 用于切换模式的 UI 组件；
   - 使用本地存储持久保存用户偏好的方式；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍访问属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

9. 返回 **Conversation** 选项卡。
10. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。如果一切正常，应该会看到检查通过。

> [!TIP]
> 想为无障碍访问工作再获取第二份意见？在 PR 评论中提及 `@copilot`，并提出类似“审查此 PR，查找其他 WCAG 问题”或“建议如何改进键盘导航”的请求。Copilot 会启动新会话来处理评论。

## 可选练习 — 继续在本地探索

在 IDE 中与智能体迭代协作是一项技能，只有反复练习才能掌握。以下是一些可以从 VS Code 发起的后续会话思路：

- 在游戏详情页添加支持者意向表单；
- 在游戏列表页实现分页；
- 为 `src/lib/` 中的数据访问辅助函数添加输入验证和错误处理；
- 扩大无障碍访问智能体的工作范围，例如审查整个网站的键盘焦点顺序。

## 总结

恭喜，你已完成 VS Code 练习！在本实验中，你：

- **使用 Playwright MCP 手动测试了功能。**你添加了 Playwright MCP 服务器，并让 Copilot 操作浏览器，在创建拉取请求前验证筛选功能。
- **使用智能体模式协调跨技术栈更改。**你在一个会话中添加了涉及客户端、服务器端和测试的筛选功能。
- **使用了自定义智能体。**你从智能体选择器中选择了专注于无障碍访问的自定义智能体，并观察它根据存储库内容实现高对比度模式。
- **管理并引导了智能体会话。**你在聊天中审查建议的更改、接受所需内容，并通过后续请求扩展会话，加入浅色模式。
- **通过拉取请求完成了工作闭环。**你发布了本地工作，并按照团队的方式端到端审查。

## 回顾和后续步骤

VS Code 练习中的必修部分到此结束。你可以在这里停止，工作坊已完成。

如果想进一步了解 Copilot 的智能体能力，其他练习系列会通过不同界面介绍相关场景：

- 💻 **[CLI 练习](../../cli/)** — 在终端中通过 Plan 和 Autopilot 模式、技能、自定义 QA 智能体、Playwright MCP、Agent Merge 和上下文感知交接，交付两个经过审查的更改。
- ☁️ **[云端智能体练习](../../cloud/)** — 专注于将议题分配给云端智能体、通过智能体页面监控会话，并异步迭代改进拉取请求。

你还可以继续构建已有成果。[awesome-copilot][awesome-copilot] 汇集了许多可用于自定义项目的说明文件、自定义智能体和技能。

如果想进行可选扩展，[可选：集成 Foundry][exercise-7]会使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge 并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
