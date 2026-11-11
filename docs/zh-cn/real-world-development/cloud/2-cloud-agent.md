---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少实施时间而长期积压的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码、添加功能等任务。完成后，它会创建一个草稿 PR，供开发人员审查。这样可以把繁琐的工作交给代理，加快开发进度，并让开发人员专注于更重要的问题。

通过 Copilot cloud agent，你将了解：

- 如何自定义代码生成环境；
- 如何确保操作安全执行；
- 明确界定问题范围的重要性；
- 如何将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要解决的技术债务。最初受聘创建网站初版的承包商没有妥善编写文档——你会发现网站完全缺少文档。首先，团队希望为应用程序中所有导出的函数添加 TypeScript 文档 (TSDoc) 注释。

此外，设计团队希望让用户更容易发现游戏。他们希望在每个游戏的详情页上显示相关游戏，也就是同一类别中的其他作品，让用户继续浏览。目前不需要完善的实现，只需要能用于用户体验 (UX) 验收测试的版本。这项工作目前是一个阻碍，但还有优先级更高的问题。

这两项任务都很容易被搁置，也很适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员继续完成其他工作；等它完成后，再回来审查结果并确认是否符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。就像与开发人员协作一样，你可以通过多种方式分配任务，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿 PR 来跟踪进度、准备环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好供你审查时，Copilot 会在 PR 中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 不会变魔术，思考仍然不可少。看似简单的操作也可能包含复杂情况，因此[请谨慎界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 编程伙伴：分阶段工作、不断学习和尝试，并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

在前面的练习中，你了解了自定义指令以及它们如何引导 Copilot。如果你已经完成过自定义指令的实践练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看一下。将任务分配给 Copilot cloud agent 前，先以只读方式检查此存储库中已有的指令文件，稍后就能辨认它们的影响。

在存储库的 GitHub Web 界面或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，重点留意 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些指令限定为 `**/*.test.ts` 文件。

在下一节中将 `Code lacks documentation` 问题分配给 cloud agent 时，观察生成的 PR 是否包含 TSDoc 注释、TypeScript 约定和注释头。这些内容来自指令文件，后续练习中审查 PR 时还会再次提到。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保一切就绪。将任务分配给 Copilot 时也是如此，因为它的工作方式类似于软件工程师。

cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过创建一个[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在代理开始工作前运行。这样，代理就能使用所需的开发工具和依赖项。此工作流已提前为本研讨会配置好，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及为唯一的 Astro 应用完成迁移和数据初始化的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可以从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为*你的步骤*设置所需的最低权限。Copilot 将使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端和应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并初始化本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来和其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。GitHub Actions 会在 Copilot 开始处理 PR 前运行它。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流是否正常，而不必等待 Copilot 运行。

## 添加文档

虽然大家都知道文档很重要，但许多项目的信息已经过时，或者完全缺少文档。这类技术债务往往长期无人处理，降低工作效率，也让代码维护和新开发人员加入团队变得更困难。Copilot 擅长编写文档，因此这是分配给 Copilot cloud agent 的理想问题。它会在后台生成所需文档。之后的练习中，你会回来审查它的工作。

1. 在新的浏览器选项卡中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 创建问题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   我们组织要求在有帮助的情况下为函数和方法添加 TSDoc 文档注释。遗憾的是，近期更新没有遵守这一标准。我们需要更新现有代码，在能够说明行为的地方添加文档注释。
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的 PR，其标题会类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建一个待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表以及 Copilot 准备完成的任务。
14. 向下滚动 PR 时间线，应该会看到 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要优势之一是能够异步执行任务，让我们可以专注于其他工作。接下来，我们会利用这一点，再分配一项任务给 cloud agent，同时着手为应用程序编写代码、添加功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不一定要花很长时间，也可以把时间用于其他工作。现在就把这项任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入 `Show related games on the game details page`。
6. 在 **Description** 中输入：

   ```markdown
   我们希望在每个游戏的详情页上显示相关游戏，帮助用户发现更多游戏。设计团队希望探索 UX 并进行验收测试。要求如下：

   - 在 `src/lib/` 中添加一个数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包括该游戏本身
   - 在游戏详情页上显示一个使用该辅助函数的“Related games”部分
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前，确保所有测试均通过
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，你会在问题的第一条评论中看到几个 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二项请求。Copilot cloud agent 的工作方式类似软件工程师，因此你不需要一直监控它；等任务完成后再审查即可。接下来，我们来创建和使用自定义代理。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将问题分配给 cloud agent，让它异步处理任务。Copilot 可以帮助解决技术债务、创建新功能，或将代码从一种框架迁移到另一种框架。

你了解了以下内容：

- 如何自定义代码生成环境；
- 如何确保操作安全执行；
- 明确界定问题范围的重要性；
- 如何将问题分配给 Copilot。

Copilot cloud agent 在后台工作时，我们可以转而创建和使用自定义代理。[Copilot cloud agent 也可以使用 Model Context Protocol (MCP) 服务器][cloud-agent-mcp]，并且可以使用自定义指令；我们已在前面的模块中探索过这些内容。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
