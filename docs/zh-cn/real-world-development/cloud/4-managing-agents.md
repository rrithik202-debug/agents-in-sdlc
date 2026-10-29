---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你已让 Copilot 云智能体处理了三项旨在改善用户体验和添加功能的任务。云智能体可以异步自主工作，但监控这些任务仍然很重要。

GitHub.com 提供了多种工具来管理分配给云智能体的任务，包括[智能体页面][agents-page]。在这个控制中心，你可以查看所有具有未关闭拉取请求（PR）的智能体任务，了解执行的操作，还可以引导正在进行的会话。

在本课中，你将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，顺便加入浅色模式是合理的。你将引导智能体，确保它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的应用程序编程接口（API）。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从仓库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置模型上下文协议（MCP）服务器。
    - 执行完成任务所需的各项步骤。

> [!NOTE]
> 根据 Copilot 的工作状态和执行方式，列出的具体步骤会有所不同。

4. 同时留意右侧显示的拉取请求面板。通过此面板可查看 PR 和更改的文件，进一步监控任务。

## 引导云智能体

现在你已查看正在进行的任务，可以请求 Copilot 在实现高对比度模式的同时添加浅色模式开关。

1. 选择与添加高对比度模式相关的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。看到类似下方的消息时，就表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。在完成你发送消息时正在执行的具体步骤后，它会将新请求纳入工作流程。

与之前一样，这需要几分钟。此时可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过此控制中心，你可以跟踪所有正在进行和已完成的任务、审查工作内容，并重定向正在进行的任务以调整范围或提供更多指导。

你探索了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
