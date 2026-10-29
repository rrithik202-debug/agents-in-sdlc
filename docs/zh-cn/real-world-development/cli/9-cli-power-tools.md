---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、查看内部信息和配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，接下来探索其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树以及委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在来看看其他能力，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在仓库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统指令、工具、消息和剩余空间之间的分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。若要自行决定压缩时机，请使用 `/compact`。若要切换到不相关的任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验有助于共同改进 AI 工具的使用方式。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布内容前，先审查导出内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的仓库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给异步工作的 Copilot 云智能体；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。尝试前先创建一个范围明确的新任务，并通过常规工作流审查结果。若要进一步了解异步智能体工作，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看内部状态。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理上下文。
- 使用 `/model` 查看可用模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和云智能体委派的可选命令。

Copilot CLI 还有更多命令和功能可供探索。接下来，[回顾已学内容][next-lesson]并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
