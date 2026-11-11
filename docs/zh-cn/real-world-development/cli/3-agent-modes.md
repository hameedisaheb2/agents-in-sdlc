---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据问题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先在项目中添加了一个小功能。但较大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。本课程是系列课程的第一部分，你将遵循典型的代理驱动开发流程：从问题出发创建功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 在继续开发此功能时，你会使用同一对话和分支。通常会按不同文件类型使用不同分支或 PR，但本研讨会会简化这些步骤，让你专注于核心概念。

在本课程中，你将：

- 从 GitHub 问题开始新的 Copilot 对话；
- 在 Plan 模式下定义要求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办问题描述了该功能，但在编写代码前，需要先确定类别组合等细节。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确的范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则。事实上，这些原则变得更重要了！大多数开发者都会遵循类似的流程：

1. 从描述待完成工作的已登记问题开始。
2. 规划需要构建的内容。
3. 编写并审查代码。
4. 运行测试来验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查完成且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述内容相似。

遵循这种标准方法，可以确保 AI 生成的代码满足要求，并经过与手动编写的代码相同的审核流程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协同工作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，编写代码、运行测试并不断迭代，直至任务完成。

先在 Plan 模式下开始并审查计划，然后使用 Autopilot 实现计划。

## 从问题开始

开始筛选工作前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，然后返回 **Session** 选项卡。

注意提示现在以 `#7` 或类似编号开头。使用 `#` 可以将 GitHub 问题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以让你在交由 Copilot 处理前，先确定如何实现功能或执行任务。对于复杂工作，最好先花些时间规划。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   请为实现此功能制定计划。
   ```

Copilot 会先探索项目，再确定合适的实现方式并制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答即可，没有唯一的正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动并审查计划。
6. 如果计划中的任何内容与你的决定不符，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查完毕，现在开始实现。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续处理问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 查看 Copilot 如何按照确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也需要在合并前审查。请审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与问题描述以及规划时做出的决定进行比较。
4. 完成代码审查后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并请 Copilot 修复失败项。

## 体验新功能

代码看起来不错，但能否运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，以便我在浏览器中试用筛选功能。告诉我 URL，并让服务器保持运行。
   ```

2. 当 Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合。
4. 确认重置和无结果时的行为符合问题描述和你的决定。
5. 返回 Codespace，并请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。本课程中，你：

- 从 GitHub 问题开始新的 Copilot 对话；
- 在 Plan 模式下定义要求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来，通过[自定义指令][next-lesson]深入了解如何让生成的代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
