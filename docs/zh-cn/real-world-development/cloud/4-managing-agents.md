---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几次练习中，你让 Copilot 云端智能体处理了三个任务，旨在改善用户体验并添加功能。云端智能体以异步和自主的方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云端智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有未关闭拉取请求（PR）的智能体任务，了解智能体执行的操作，甚至引导正在进行的会话。

本课将学习如何：

- 使用智能体页面监控云端智能体任务；
- 引导正在运行的会话，为其添加功能要求。

## 场景

分配高对比度模式任务后，团队发现也可以借此添加浅色模式。既然正在更新网站样式并添加切换功能，加入此功能也很合理。你将引导智能体的工作，让它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云端智能体任务

查看当前分配给 Copilot 云端智能体的所有任务的状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该会看到已分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码；
    - 创建 Copilot 工作所需的环境；
    - 设置 MCP 服务器；
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体列出的步骤会因 Copilot 工作状态和所采用的方法而异。

4. 同时查看右侧显示的拉取请求窗格，以便查看 PR 和已更改的文件，进一步监控工作进度。

## 引导云端智能体

你已了解当前正在进行的任务。现在，我们将要求 Copilot 在处理高对比度模式的同时，也加入浅色模式切换功能。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始执行任务。看到类似以下消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中突出显示“Steer active session while Copilot is working”对话框的任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 如何确认收到提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成收到消息时正在执行的步骤，然后将新要求纳入工作流程。

这需要几分钟。你可以趁此机会暂停一下，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云端智能体任务的中心位置。你可以使用该任务控制中心跟踪所有正在进行和已完成的任务、审查执行中的工作，并重定向正在运行的任务以调整范围或提供补充指导。

你了解了以下内容：

- 使用任务控制中心和智能体页面监控云端智能体任务；
- 重定向正在运行的会话，要求添加功能。

Copilot 正在完成无障碍访问功能。接下来，我们将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
