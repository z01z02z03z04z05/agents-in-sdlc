---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义配置和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的完整工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题；
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式中制定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查；
- 使用自定义说明引导智能体，然后自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加了 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中探索筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估要求、测试覆盖率、技能结果和浏览器验证证据；
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 已完成的交付内容

本工作坊分为两个 PR 阶段，分别在基于最新 `main` 的独立分支上完成：

1. **星级评分：**在游戏卡片上显示已有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你使用了同一对话和分支。为简化工作坊流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：运行自动化测试、在浏览器中自行检查，并通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在审查要求和测试覆盖率时也整合了这些结果。新增测试应能弥补实际缺口；如果 QA 确认无需添加测试，也可能是合理的。合并前请审查代码和证据，更改后应更新受影响的验证证据。

## 最佳实践

为 Copilot 提供的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并设置了 MCP 服务器。可以在不同对话中复用这些自定义设置，并根据团队需求进行调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。请审查实际更改和工具结果，而不要只看智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前考虑方案，使用 **Interactive** 持续参与重点更改，使用 **Autopilot** 处理范围明确的任务。日常编辑可以选择速度较快的模型，复杂工作则选择能力更强的模型。

上下文仍然和基础设施同样重要。清楚说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出结果。

## 更多探索

你已学习核心工作流。还可以了解以下 CLI 功能：

- `/review`：让代码审查智能体分析更改；
- `/rubber-duck`：讨论问题并听取其他观点；
- `/fleet`：协调多个独立子任务并行执行；
- `/worktree`：隔离单独的任务；
- `/delegate`：将任务发送给 Copilot 云端智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式！可以将其用于生产代码、个人项目，或多年来一直想做却没时间实现的小应用。与团队分享经验，也向他们学习。和往常一样，也要继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot 应用工作坊][app]或 [Copilot 云端智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
