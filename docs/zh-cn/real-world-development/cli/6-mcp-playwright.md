---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如之前所说，编写代码不只是写代码。我们需要处理数据、外部服务，甚至让 Copilot 使用其他自动化工具。MCP 服务器正是为此而设计的。它能扩展 Copilot CLI 的内置功能，为 Copilot 提供更多工具和服务。

本课将学习如何：

- 了解模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 让智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新需要实际与 UI 交互。你希望让 Copilot 像用户一样使用正在开发的网站，进一步自动化更改验证流程，从而更有把握地确认更新符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可以访问的一组工具和资源。

以下是一些常用的 MCP 服务器：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，便于发现和参与生态系统。

> [!CAUTION]
> 请像对待项目中其他依赖项一样对待 MCP 服务器。使用前应审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未安装 Playwright MCP 服务器，请先添加它，让 Copilot 能够像用户一样与网站交互。

> [!NOTE]
> 如果已在 Copilot CLI 中配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在，我们用 Copilot 中的 Playwright MCP 服务器完成同样的验证！

1. 使用以下提示词，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确告诉 Copilot 使用 MCP 服务器，它会自行判断。不过，既然你知道应该使用哪个服务器，明确指出也有帮助！这样能让结果更一致，也能节省一些令牌。

2. 观察 Copilot 列出并在浏览器中执行的各个步骤，确认功能是否正常。
3. 阅读报告，确认一切是否符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并向你报告结果。

## 总结和下一步

恭喜，你已通过 Copilot CLI 在真实浏览器中使用 Playwright MCP 服务器探索功能！回顾一下，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它；
- 在尚未配置时添加了 Playwright MCP 服务器；
- 让智能体驱动浏览器并探索筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
