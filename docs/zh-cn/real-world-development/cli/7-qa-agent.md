---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题要求、quality-checks 技能和 Playwright MCP 整合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中查看筛选功能。现在，我们将在自定义智能体中整合这些能力，并为 QA 定义清晰的流程。

本课将学习如何：

- 探索自定义智能体如何结合说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在打开拉取请求（PR）前，针对要求、代码质量、自动化检查、测试覆盖率和浏览器行为执行一致的审查流程。自定义智能体可以协调该 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。该配置文件会说明智能体的用途、指导内容和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

之前使用的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力检查要求并报告结果。它不会替代这些功能，也不需要另开对话。

## 创建 QA 配置文件

打开功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。该配置文件会说明 QA 执行哪些检查，以及必须遵守哪些边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 已准确记录预期 QA 工作流和边界。这样可以避免智能体在你只希望验证功能时，却因配置不完整或范围过广而擅自更改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读前置元数据。`description` 是必填项；`name` 可选，但添加后可以为智能体指定清晰的显示名称。
3. 阅读配置文件中的说明，确认 QA 会从要求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在更改实现代码前征求许可，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请让默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选对话并选择 QA，以便它使用已有的议题和规划决定。

1. 使用以下提示词启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能暂时不会出现在可用智能体列表中，但可以通过上面的命令启用。

2. 使用以下提示词，让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体开始工作。
4. 完成后，阅读它提供的报告。

## 总结和下一步

你已在工作流中添加可复用的专业角色，并审查了其工作。本课中，你：

- 了解了自定义智能体如何结合说明、技能和 MCP 工具；
- 创建并检查了可复用的质量保证配置；
- 选择了 QA 智能体，并根据筛选议题审查了它的发现。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备就绪，可以进行审查。接下来，将它们[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
