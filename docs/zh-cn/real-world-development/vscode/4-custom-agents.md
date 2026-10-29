---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的专用人工智能（AI）助手。通过在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，你可以为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编码为可复用的智能体，例如执行 [Web 内容无障碍指南（WCAG）][wcag] 合规要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存放在 `~/.copilot/agents`。每个文件都包含 YAML（YAML Ain't Markup Language）frontmatter，至少包含 `name` 和 `description`，后面是用于定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并指导 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行者，**技能**是工具。

自定义智能体有独立的上下文窗口，并用于协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；在此过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 的其他用法一样，需要通过测试和迭代，找到最适合自身工作环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文字与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现一种可由用户开关的高对比度模式。

无障碍功能至关重要，因此需要尽快实现。你将使用自定义智能体生成此功能。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

我们已为你创建了无障碍自定义智能体。现在查看其内容，了解它如何引导 Copilot。

先从 `main` 创建无障碍功能分支，将这项工作与练习 3 中的筛选功能拉取请求（PR）分开。

1. 返回 codespace，打开终端并创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> `name` 和 `description` frontmatter 是自定义智能体的必需项。

3. 接着浏览并审查后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该会话会使用其指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同；需要时会在相应步骤中说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请选择它。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（其中显示当前智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作，探索代码库、提出更改建议并修改项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可供审查的差异。
6. 随着编辑逐步完成，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改仓库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的专用 AI 助手。使用自定义智能体可以将团队专业知识和标准整理成可复用的智能体，帮助 Copilot 更有效地完成特定类型的工作。

你探索了以下内容：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，在更改进行时审查，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在集成开发环境（IDE）中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
