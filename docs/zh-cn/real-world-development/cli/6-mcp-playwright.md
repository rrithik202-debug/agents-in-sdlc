---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码不只是实现代码，还要处理数据、外部服务，并为 Copilot 提供更多自动化能力。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

在本课中，你将：

- 了解模型上下文协议（MCP）及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未配置时添加它。
- 请智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要实际与界面交互。你将让 Copilot 像用户一样使用网站，进一步自动验证更改是否符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。通过 MCP，智能体可以实时获取最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器都提供一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，方便发现和参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 中的 Playwright MCP 服务器重复验证。

1. 输入以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照议题和计划检查筛选功能。告诉我哪些正常、哪些不正常，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常无需明确指定 MCP 服务器，Copilot 会自行判断。不过，指出要使用的服务器有助于保持结果一致。

2. 观察 Copilot 在浏览器中执行的各项验证步骤。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器并提供报告。

## 总结和后续步骤

恭喜，你已在 Copilot CLI 中使用 Playwright MCP 服务器通过真实浏览器体验功能。本课中，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未配置时添加了它。
- 请智能体驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
