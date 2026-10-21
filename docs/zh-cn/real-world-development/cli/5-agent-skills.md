---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证了代码，也使用说明文件确保代码遵循标准。但测试、lint 以及持续集成（CI）的其他部分又该如何处理？

**智能体技能**非常适合此类任务！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将学习如何：

- 探索现有的 `quality-checks` 技能；
- 自定义其结果格式；
- 重新加载并运行该技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告格式，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，供智能体按需加载。技能的核心是一个同名文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件包含 `name` 和 `description` 前置元数据，用于定义技能，并包含技能功能概述和调用时机指导。技能文件夹还可以包含脚本及其他资源的子文件夹，供调用技能时使用。

> [!NOTE]
> 技能不一定要包含其他文件夹和文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

我们来查看 Tailspin Toys 团队创建的 `quality-checks` 技能，了解它如何运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述帮助 Copilot 判断何时调用该技能。
3. 阅读说明，留意它们如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能中还没有 **Results output formatting** 部分。

## 修改前先运行技能

技能可以通过 Copilot CLI 中的斜杠命令或自然语言直接调用。现在让 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选对话。
2. 使用以下提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望获得一份更好的报告，能够说明运行了什么、是否成功，以及工具实际报告了哪些结果。现在就更新技能，让它生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看实际效果！Copilot CLI 可以在不重启对话的情况下重新加载修改后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告，并与第一份报告比较。

## 总结和下一步

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果报告格式；
- 重新加载并运行了该技能。

这一更改会与筛选功能一起纳入功能 PR。接下来，我们将让 Copilot [通过 Playwright MCP 服务器直接与网站交互][next-lesson]。

## 更多技能示例

这些社区示例可供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
