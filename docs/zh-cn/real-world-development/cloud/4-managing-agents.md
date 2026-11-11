---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前面的两个练习中，你让 Copilot cloud agent 承担了三项任务，重点是改善用户体验和添加功能。cloud agent 虽然可以自主、异步地工作，但仍然需要监控这些任务。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制台中，你可以查看所有具有开放拉取请求 (PR) 的代理任务、了解它执行的操作，甚至引导正在运行的会话。

在本练习中，你将：

- 浏览代理页面，监控 cloud agent 的任务；
- 引导正在运行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以趁此机会添加浅色模式。由于已经在更新网站样式并添加模式切换功能，将浅色模式一并加入很合适。你将引导代理同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 的任务

先查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 转到 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
   - 更新代码库文档；
   - 生成用于修改游戏的应用程序编程接口 (API)；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 创建 Copilot 的工作环境；
   - 设置 Model Context Protocol (MCP) 服务器；
   - 执行完成任务所需的各项操作。

> [!NOTE]
> 实际显示的步骤会因 Copilot 的工作状态和采用的方法而异。

4. 还要查看右侧的 PR 面板。你可以在那里查看 PR 和已更改的文件，以便继续监控。

## 引导 cloud agent

现在你已经查看了正在进行的任务，可以让 Copilot 在实现高对比度模式时一并添加浅色模式。

1. 选择与高对比度模式相关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

   ![控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。出现类似以下消息时，就表示已经开始。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   在实现高对比度模式的同时，也添加浅色模式。这个模式也应该有一个开关，让用户选择自己想要的显示模式。
   ```

   ![代理页面中的 cloud agent 任务截图，突出显示了引导正在运行的会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会在完成你发送消息时正在执行的步骤后，将新请求加入工作流程。

这同样需要几分钟。你可以趁此休息一下，并回顾目前学到和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot 代理页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以跟踪所有正在进行和已完成的任务、审查代理正在执行的工作，也可以重定向正在运行的会话来调整范围或提供更多指导。

你了解了以下内容：

- 使用控制台和代理页面监控 cloud agent 的任务；
- 重定向正在运行的会话以请求添加功能。

Copilot 正在完成无障碍功能的工作，接下来你可以[迭代改进 Copilot 创建的 PR][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
