---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将问题要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个自定义代理中，并为它定义清晰的质量保证 (QA) 流程。

在本课程中，你将：

- 了解自定义代理如何结合指令、技能和 MCP 工具；
- 创建并检查可重复使用的 QA 配置文件；
- 选择 QA 代理，并根据筛选问题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并提供可重复使用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力来评估要求并报告发现。它不会取代其他功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建一个可重复使用的 QA 配置文件。该配置文件会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   在 `.github/agents/qa.agent.md` 中创建名为 QA 的自定义代理。它应根据问题和已商定的要求检查功能、遵循存储库指令、运行 `quality-checks` 技能、使用 Playwright MCP 验证行为，并在覆盖范围不足时添加测试。

   要求它为每项要求报告 `pass`、`fail` 或 `blocked` 状态，并提供相应证据。修改实现代码前必须先询问，而且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新代理前，先检查配置文件，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免你只要求验证时，一个不完整或范围过大的代理却修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需项；`name` 是可选项，但添加它可以为代理设置明确的显示名称。
3. 阅读配置文件指令，确认 QA 从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请先让默认代理修改，再继续。

## 根据问题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一个筛选对话并选择 QA，这样它就能使用上下文中的问题和规划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建代理，它可能尚未出现在可用代理列表中。代理已创建，上述命令会启用它。

2. 使用以下提示请 QA 代理审查功能：

   ```plaintext
   请根据问题和计划中确定的事项审查筛选功能。它可以创建 PR 了吗？
   ```

3. 等待 QA 代理完成工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加了可重复使用的专业角色，并审查了它的工作。本课程中，你：

- 了解了自定义代理如何结合指令、技能和 MCP 工具；
- 创建并检查了可重复使用的质量保证配置文件；
- 选择了 QA 代理，并根据筛选问题审查其发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好接受审查。接下来，[将它们汇总到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
