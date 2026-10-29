---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少实施时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求（PR），供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

你将在本练习中探索 Copilot 云智能体如何：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 认识清晰定义议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受聘开发网站的承包商留下的文档状况不佳——实际上，完全没有文档。首先，团队希望为应用程序中所有导出的函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页显示相关游戏，也就是相同类别的其他游戏，让用户可以继续浏览。当前不需要完善的实现，只需要一个可用于用户体验验收测试的版本。虽然这项工作目前受阻，但还有优先级更高的议题。

这两项任务都可能很快被降低优先级，因此很适合分配给 Copilot 云智能体。云智能体可以异步处理任务，让开发人员先处理其他工作，稍后再回来审查 Copilot 的工作并确认结果符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与人工开发人员协作类似，也有多种方式与它配合，其中包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看详情。建议方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 清晰定义指令范围的重要性

GitHub Copilot 并非魔法，不能替你省去思考过程。即使看似简单的操作，深入分析后也可能很复杂，因此[请谨慎确定 Copilot 云智能体任务的范围][cloud-agent-best-practices]。将它视为人工智能（AI）结对程序员：分阶段工作、学习、试验并逐步调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本仓库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未完成，现在正好快速查看一下。将工作分配给 Copilot 云智能体之前，只读查看仓库中已有的指令文件，以便稍后观察它们的影响。

在仓库的 GitHub 网页界面中打开以下文件，或者在已启动的 codespace 中查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其留意 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。这些指导来自上述指令文件，稍后审查 PR 时还会再次提及。

## 设置 Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境状态良好。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师（SWE）的方式执行任务。

云智能体使用 [GitHub Actions][github-actions] 环境开展工作。你可以创建专用的设置工作流来定制环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在智能体开始工作前运行。这样可让它访问所需的开发工具和依赖项。为简化实验流程并提供学习机会，本实验已预先配置好此工作流。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件类似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理拉取请求之前通过 GitHub Actions 执行。
- `workflow_dispatch` 触发器可让你在仓库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目中的文档要么过时，要么缺失。这类技术债务常常无人处理，降低工作效率，也增加维护代码库或引入新开发人员的难度。Copilot 擅长创建文档，因此这是分配给 Copilot 云智能体的理想议题。它会在后台生成所需文档；之后的练习会回来审查它的工作。

1. 在新的浏览器标签页中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建工作环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表以及 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们可以专注于其他工作。接下来利用这一特性，再给 Copilot 云智能体分配一项任务，然后转而为应用编写新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，而它处理另一组任务。为设计团队添加相关游戏部分可能用不了很长时间，但这段时间仍可用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新建议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求。Copilot 云智能体的工作方式类似软件工程师（SWE），因此你无需持续监控，任务完成后再审查即可。现在继续创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以将议题分配给 Copilot，让它异步处理工作。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 清晰定义议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并支持我们在前面模块中探索过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
