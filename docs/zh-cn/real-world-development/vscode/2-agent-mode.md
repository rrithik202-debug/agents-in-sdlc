---
title: "练习 2 - 使用 Copilot Agent Mode 添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发人员，你的工作流程通常包括：查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再次更新……还有很多类似步骤。

Copilot Agent Mode 正是为此而设计的。

Copilot Agent Mode 可在集成开发环境（IDE）中更自主地工作。它的行为类似开发人员：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在来看看如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但智能体模式和云智能体针对的是两种不同体验。智能体模式在 IDE 中执行任务，可快速反馈并与用户交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你交流。

在本练习中，你将了解如何：

- 使用 Copilot Agent Mode 探索项目并找出相关文件，协调多文件更改。
- 使用 GitHub Copilot Agent Mode 跨用户界面（UI）和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解现有功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，用户可以查看游戏列表及其详情。网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射器（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能通过标准文档对象模型（DOM）API 和小型、作用域受限的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应该在 `main` 分支上。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的提示，类似于：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，在终端中选择 localhost URL 会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，Web 服务器目前正运行在该 Codespace 中。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表及其标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示该游戏的更多信息，包括标题、说明、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同；需要时会在相应步骤中说明。

网站的初始实现已经可用，但我们想添加新功能。先查看待办事项。通过模板创建项目时，系统已自动生成一组议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以提供良好体验。不过，由于大语言模型（LLM）的结果具有概率性，你看到的建议可能与说明中的内容不同。这完全正常，符合预期。

> [!NOTE]
> 由于结果具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行命令列出所有议题。
5. 查看生成的议题列表。

Copilot 甚至会根据它认为对用户最有用的程度为议题排序。

## 查看指令文件

让智能体生成代码之前，先查看用于向 Copilot 提供上下文的指令文件。这里将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含向网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样便于重复使用内容，将复杂指令拆分成较小的可管理部分，并引用示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用做三项独立更改：

- 添加或完善数据层 (`src/lib/`) 中剩余的筛选逻辑
- 添加或更新筛选行为测试
- 更新游戏列表页，添加筛选 UI

此外，合并前需要运行测试并确保测试通过。Copilot Agent Mode 可以代你完成这些任务，现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到与目标功能相关的文件。你会看到它查找数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，可能需要几分钟。在此过程中，编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作进行时选择。

7. 按 Copilot 提示选择 **Continue**，运行测试。

   ![Copilot Chat 面板提示用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot Agent Mode 完整 Chat 会话的截图，显示上下文、大语言模型（LLM）和工具的交互](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot Agent Mode 跨 UI 和数据层实现新功能。
- 使用 Copilot Agent Mode 探索项目、找出相关文件并协调多文件更改。
- 在将代码合并到代码库之前，审查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习 – 实现分页

随着游戏列表增长，网站将需要分页功能。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写提示时可考虑以下方面：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页的实现方式：允许用户选择页面大小，还是使用固定页面大小。
- 编写提示时提供必要指导，以便 Copilot 按期望实现功能。
- 你可能需要与 GitHub Copilot 迭代，要求更改并提供上下文。这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
