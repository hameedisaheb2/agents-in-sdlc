---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[**GitHub Copilot cloud agent**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) 可以让 GitHub Copilot 在云端异步工作。你可以在 GitHub 上分配任务，cloud agent 会在后台处理：探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

在这些练习中，你将添加 cloud agent 要遵循的自定义指令，然后分配一个 GitHub 问题让它实现。你还将审查和使用自定义代理来调整工作方式，通过代理仪表板监控和引导会话，最后审查代理创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 要遵循的自定义指令 |
| [2. Cloud agent][ex2] | 异步代理 | 将问题分配给 GitHub Copilot cloud agent |
| [3. 自定义代理][ex3] | 专用代理 | 审查和使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代改进][ex5] | 审查 | 审查拉取请求、迭代改进 Copilot 的工作并选择后续步骤 |

## 准备工作

参加此研讨会前，请确保具备以下条件：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 GitHub Copilot cloud agent

> [!TIP]
> 没有付费计划？通过验证的学生可以使用 [GitHub Education][callout-student-plan-education] 免费获取 GitHub Copilot。**Copilot Student** 计划包含本研讨会所用的代理、MCP、代码审查和 Copilot CLI 功能，因此可以完成所有环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise** 计划，但不适用于 Copilot Free。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用该功能。

> [!NOTE]
> 本环境不介绍 MCP。若要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 环境][cli]或 [VS Code 环境][vscode]。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
