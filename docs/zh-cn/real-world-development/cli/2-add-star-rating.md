---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在开始对项目进行第一次更改。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将让智能体显示评分、审查更改，并通过第一个拉取请求合并它。

在本课中，你将：

- 在功能分支上启动专注于某项任务的 Copilot 对话。
- 请智能体对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，详情页已经显示评分，但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让智能体在每张卡片上显示现有评分。这是适合首次会话的小型独立更改。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的仓库和 Git 分支，而不是创建专用工作树。保存的对话可供日后继续使用，但文件和分支仍是磁盘上的普通 Git 状态。

在对话中，你会看到提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

新建对话以开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保启动新会话。
5. 输入以下提示：

   ```plaintext
   在列表页的游戏卡片中显示每款游戏的 5 星制 starRating。如果评分为 null，则显示“No rating yet”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情的文件并编写所需代码。现在，你已使用 Copilot CLI 添加了一项功能。

## 审查差异

合并前应审查所有 AI 生成的更改，包括小型更改。现在直接在 Copilot CLI 中检查它们。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果；如有失败，请让 Copilot 修复。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具具有概率性，因此生成的代码可能与你的不同。请检查行为，不要期待某种完全一致的实现。

## 检查更改

不要只阅读代码就假定它能正常运行。请 Copilot 启动网站，然后在 Codespaces 转发的浏览器中查看更新后的界面。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了 5 星制评分。
4. 返回 Copilot，并请它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

现在功能已完成，可以创建拉取请求（PR），将新代码合并到项目中。

1. 请默认智能体提交已审查的更改：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Copilot CLI 显示的 URL 以打开 PR。
4. 审查已更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，你已使用 GitHub Copilot CLI 发布了第一个更改。你完成了以下工作：

- 在功能分支上启动专注于某项任务的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并第一个拉取请求。

接下来，你将[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
