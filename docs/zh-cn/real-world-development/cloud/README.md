---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot 云端智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可在云端异步执行工作。你可以在 GitHub 上分配任务，云端智能体会在后台接手工作：探索存储库、进行更改并创建拉取请求；此时你可以继续处理其他工作。

在这些练习中，你将添加云端智能体要遵循的自定义说明，然后分配一个 GitHub 议题并让它实现所需更改。你还将审查并使用自定义智能体来指导其工作方式，从智能体仪表板监控并引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云端智能体要遵循的自定义说明 |
| [2. 云端智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云端智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作，并选择后续步骤 |

## 先决条件

参加此工作坊前，请确保你具备以下条件：

- [ ] 拥有 GitHub 账户，并使用有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 已为你的账户或组织启用 Copilot 云端智能体

> [!TIP]
> 没有付费计划？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云端智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise** 计划，不适用于 Copilot Free。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用该功能。

> [!NOTE]
> 本系列练习不涉及 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 练习][cli-harness]或 [VS Code 练习][vscode-harness]。

## 开始使用

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
