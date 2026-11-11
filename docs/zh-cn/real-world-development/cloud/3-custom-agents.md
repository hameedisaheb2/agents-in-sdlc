---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 的[自定义代理][custom-agents-concept]可以创建专门处理开发工作流中特定任务或领域的人工智能 (AI) 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义代理，可以为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更高效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的代理，例如确保符合 Web 内容无障碍指南 (WCAG) 标准的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理定义在项目 `.github/agents` 文件夹中的 Markdown 文件里，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML (YAML Ain't Markup Language) frontmatter，至少包含 `name` 和 `description`，后面是用于定义代理行为、专业能力和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]存在一些重叠：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区别是：**自定义代理**负责工作，**技能**是它使用的工具。

自定义代理有独立的上下文窗口，并负责协调技能，甚至其他代理来完成工作。在本练习中，无障碍自定义代理会根据无障碍指南审查和更新网站；在此过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 创建自定义代理没有唯一的正确方法。和 AI 的其他用法一样，需要不断测试和迭代，找到适合当前环境和场景的做法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

通过自定义代理，你将了解：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期的用户反馈指出，由于文字和背景的对比度不足，一些用户觉得当前深色主题难以阅读。为解决这一无障碍问题，设计团队要求实现可供用户切换的高对比度模式。

无障碍功能至关重要，因此你希望尽快实现它。接下来将使用自定义代理生成此功能。

## 检查无障碍自定义代理

无障碍自定义代理已准备好。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例；
   - 常见问题和错误。

## 创建并分配问题

代理控制台是你管理当前环境中所有代理的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，也可以重定向正在运行的会话并提供更多指导。现在先将创建高对比度模式的任务分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建新问题。
5. 在 **Title** 中输入 `Add high contrast mode to website`。
6. 在 **Description** 中输入：

   ```plaintext
   我们需要为网站添加高对比度模式。应该提供一个开关，供用户启用该模式，并将设置保存在浏览器的 local storage 中。
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

   ![cloud agent 分配界面的截图，其中自定义代理和无障碍选项已突出显示](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 的[自定义代理][custom-agents]，也就是专门处理特定任务和领域的 AI 助手。借助自定义代理，你可以将团队的专业知识和标准整理成可重复使用的代理，引导 Copilot 更高效地完成特定工作。

你了解了以下内容：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式，你可以转到代理控制台[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为在组织中使用自定义代理做好准备][org-custom-agents]
- [为在企业中使用自定义代理做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
