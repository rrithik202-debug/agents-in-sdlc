---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，将议题要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，建立清晰、可复用的 QA 流程。

在本课中，你将：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，对照筛选议题审查结果。

## 场景

创建拉取请求（PR）前，Tailspin Toys 希望一致地检查要求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调这一 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的专用 Copilot。配置文件说明智能体的用途、指令和可用工具。本研讨会会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义项各有用途：仓库指令说明团队规范；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何运用这些能力检查要求并报告发现。它们彼此互补，无需启动另一段对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。该配置将定义 QA 要执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据议题和已确认的要求检查功能，遵循仓库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它通过 pass、fail 或 blocked 报告每项要求，并提供支持证据。修改实现代码前必须先询问我，且不得提交更改或创建拉取请求。

   目前只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免在只需要验证时生成不完整的配置或擅自更改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 可选，但添加后能清楚显示智能体名称。
3. 阅读配置指令，确认 QA 会从要求开始，遵循仓库指令，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建 PR。
5. 如果生成的配置遗漏任何职责或边界，请先让默认智能体修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后，恢复同一筛选对话并选择 QA，以便使用已有的议题和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但仍可通过上述命令启用。

2. 输入以下提示，请 QA 智能体审查功能：

   ```plaintext
   请对照议题和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. 等待 QA 智能体完成工作。
4. 阅读它生成的报告。

## 总结和后续步骤

你添加了一个可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，对照筛选议题审查了结果。

现在实现、技能更新、QA 配置、测试和验证报告都已准备好进行审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
