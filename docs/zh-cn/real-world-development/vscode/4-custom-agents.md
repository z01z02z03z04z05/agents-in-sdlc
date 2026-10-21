---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可以针对开发工作流中的特定任务或领域，创建专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，为 Copilot 提供专门的说明、最佳实践、编码模式和领域知识，从而更有效地引导它完成特定类型的工作。团队可以将专业经验整理成可复用的智能体，例如确保符合 [WCAG][wcag] 标准的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少有 `name` 和 `description` 字段的 YAML 前置元数据，后面则是定义智能体行为、专长和指导内容的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是工作者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并且会协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍访问自定义智能体会根据无障碍访问指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 创建自定义智能体没有唯一的正确方法。与 AI 中的其他做法一样，请在自己的环境和场景中测试并迭代改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。近期用户反馈指出，由于文本和背景颜色对比度不足，一些用户难以阅读当前的暗色主题。为解决这一无障碍访问问题，设计团队要求实现一种可由用户切换的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。你将使用自定义智能体生成该功能。

在本练习中，你将：

- 查看现有的无障碍访问自定义智能体；
- 在 Copilot Chat 中使用无障碍访问智能体实现高对比度模式。

## 查看无障碍访问自定义智能体

我们已为你创建了无障碍访问自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端并从 `main` 创建一个新分支，用于独立处理无障碍功能（与练习 3 中的筛选 PR 分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意其中包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> `name` 和 `description` 前置元数据是自定义智能体的必填项。

3. 接着浏览并查看以下内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍访问最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。你可以选择一个自定义智能体，让聊天会话遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段干净的对话。模式和模型因练习而异；适用时，每一步都会说明如何选择。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（此处会显示当前智能体，默认是 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍访问智能体发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议，并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 智能体提交编辑后，**Files changed** 指示器会更新，显示本次会话中智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一课中，你将审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域的专用 AI 助手。借助自定义智能体，可以将团队的专业知识和标准整理为可复用的智能体，更有效地引导 Copilot 完成特定类型的工作。

你了解了以下内容：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并引导智能体的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
