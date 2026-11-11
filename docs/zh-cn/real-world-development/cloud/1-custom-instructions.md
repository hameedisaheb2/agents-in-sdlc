---
title: "练习 1 - 自定义指令（cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它可以获取这些上下文。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过存储库自定义指令和路径范围指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会运行前后对比提示。Copilot cloud agent 会异步处理 GitHub 问题，因此难以实时并排展示效果。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 和优秀的开发团队一样，对开发实践有一套指南和要求，包括：

- 数据层必须始终有单元测试。
- 用户界面应使用深色模式并呈现现代风格。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加注释块，说明文件的用途。

使用指令文件可确保 Copilot 获取正确的信息，并按照这些实践执行任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库以及想在代码中使用的注释类型。你可以为整个存储库或特定文件类型创建指令，为具体任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个单独的指令文件，会随存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，即大多数聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和其他全局指南。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。你可以用它们为 TypeScript 或 Astro 等语言提供指南，也可以为创建用户界面组件或单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持采用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件不属于本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指南，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指令。
- 使用自然语言，保持指南清晰，并提供代码应如何编写以及应避免哪些做法的示例。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过尝试找到最适合项目的做法。

> [!TIP]
> 使用 GitHub Copilot 的每个项目都应有一套完善的指令文件。浏览本项目的文件时，你会发现其中有许多任务的指令，包括[用户界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。每种使用环境提供的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关环境的课程会在适当位置介绍这些功能。
>
> 如果需要模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中汇集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义指令文件

此存储库包含一个核心 `copilot-instructions.md` 文件，以及多个面向不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web 界面中打开并查看这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 中，留意嵌套的 **GitHub Actions Workflows** 指南。这些指南适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用于哪些文件。在这里，符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看本项目中创建单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）以及项目现有文件的链接。这样可以将较大的指令集拆分成更小且可重复使用的文件，并向 Copilot 提供生成代码时可参考的示例。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件头注释的规则。

## 添加存储库级标准

接下来要进行本练习中唯一的一处编辑：添加一条适用于整个项目的规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本环境中，cloud agent 处理你分配的问题时会采用这条指令。

编辑前先创建工作分支。cloud agent 会从问题目标分支读取指令，但你仍会单独提交这些更改。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。请记录存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加注释块来说明文件的用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指南，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指令。存储库级文件适合放置项目通用标准，例如刚刚添加的文档注释规则。

## 提交、推送并合并指令更新

cloud agent 会从问题目标分支读取指令文件。在下一练习中，将问题分配给 Copilot 时，它会从 `main` 创建分支。因此，必须先将更改合并到 `main`，cloud agent 才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中，cloud agent 处理分配的问题时会读取这些指令。

> [!TIP]
> 如果本研讨会中要直接在 `main` 上工作，可以跳过创建分支，直接提交到 `main`。此处保留分支步骤是为了演示在实际项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目的指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。在本环境的后续课程中，你会看到 cloud agent 创建的拉取请求如何应用此标准。

接下来，[将第一个问题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
