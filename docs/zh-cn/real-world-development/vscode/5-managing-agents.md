---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

让 GitHub Copilot 进入智能体模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以在聊天中查看每个差异，接受或拒绝单项更改，还可以发送后续提示词来完善或扩展工作，而无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍访问智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行中引导智能体，在高对比度模式旁边添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍访问自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一个要求：他们还希望为觉得默认主题太暗的用户添加浅色模式切换。与其从头开始新的对话，不如引导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括它探索的文件、运行的命令和提出的编辑。每项文件编辑都会直接显示在聊天中，并提供差异以及 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容观察每次工具调用。你会看到文件读取、编辑和它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会直接显示在聊天中：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求许可（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你愿意执行的命令。

5. 等待智能体完成高对比度模式的实现。它应该会显示总结所做更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式已实现，现在可以在同一对话中添加浅色模式切换。继续同一聊天会话后，智能体会保留之前工作的上下文，包括它修改的组件、采用的样式方式和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已有更改并提出添加浅色模式的修改。
3. 像审查高对比度模式一样，审查每一项新编辑。使用 **Keep** 或 **Undo** 处理每个差异。
4. 如果智能体的实现方向不符合预期（例如，它试图以你不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示词。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示词要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换控件，把浅色模式整合到现有设置面板中”），或扩展范围（“同时为两个切换控件添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查完整的更改文件列表。你应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认实现遵循自定义智能体中的无障碍访问模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。你将在[下一课][next-lesson]发布这些更改。

## 总结和下一步

你使用 Copilot Chat 智能体模式监控并引导正在运行的会话，逐项审查文件编辑、接受或拒绝更改，并发送后续提示词扩展工作，同时保留已有上下文。

本课中，你了解了：

- 实时监控智能体的工作；
- 接受或拒绝单项文件编辑；
- 使用后续提示词引导正在进行的会话。

在[下一课][next-lesson]中，你将以拉取请求的形式发布无障碍访问工作，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
