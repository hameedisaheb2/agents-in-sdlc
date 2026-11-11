---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，请先准备好所需环境。你将创建 Tailspin Toys 存储库的副本，启动一个用于开发的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 可以正常运行。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板存储库][template-repository]创建一个实例。新实例包含实验所需的全部文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面中选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建存储库。

   ![模板存储库设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办列表已准备就绪**
>
> 从模板创建存储库时，系统会自动创建一个 GitHub 问题待办列表。整个研讨会都会使用这些问题，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

   ![Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮来创建 Codespace。

   ![创建 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，你可以探索 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到此 Codespace。现在请将其保留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 Codespace 或本地[开发容器][dev-containers]中运行。两种环境都已安装顺利完成实验所需的全部组件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**。VS Code 将构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

要使用 GitHub Copilot Chat 代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时，通常已安装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开 **Extensions** 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能需要使用 GitHub 帐户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入 `Hello world` 等消息，然后按 <kbd>Enter</kbd>。这会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。请按照提示完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口会显示出来。

## 总结

你已创建实验存储库的副本，并开始创建 Codespace。开始编写代码时会使用此环境。

## 后续步骤

开始使用 Copilot。继续学习[练习 1：自定义指令][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
