---
slug: zh-cn/real-world-development/cli
title: "GitHub Copilot CLI"
description: "在学习 Copilot CLI 模式、自定义项、MCP 工具和拉取请求自动化的同时，构建、验证并交付 Tailspin Toys 的两项更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** 将 GitHub Copilot 作为智能体编码助手带入终端。它可以探索代码库、生成代码、运行命令并连接外部工具，全部通过命令行完成，无需切换到图形化编辑器。

本研讨会将带你完成 Tailspin Toys 的连续开发流程：

1. 在 GitHub Codespaces 中准备项目、安装 Copilot CLI 并熟悉项目。
2. 添加星级评分，在浏览器中检查结果，并手动合并第一个拉取请求（PR）。
3. 从筛选议题开始，在 Plan 模式下确定方案，使用 Autopilot 模式实现，再在 Interactive 模式下审查。
4. 更新仓库指令并将其应用于筛选功能。
5. 自定义现有的 `quality-checks` 技能并用它运行项目检查。
6. 添加 Playwright 模型上下文协议（MCP）服务器，在浏览器中查看筛选行为。
7. 创建质量保证（QA）自定义智能体，审查要求、覆盖率和验证证据。
8. 审查完整的筛选更改，并使用 Agent Merge 处理筛选 PR。
9. 探索用于管理上下文、模型、共享和可选云委派的实用斜杠命令。

为了让研讨会内容更集中，你将创建两个 PR：一个用于星级评分，另一个用于筛选功能以及相关指令、技能、QA 配置和测试。筛选及质量工作流共用一个对话和分支，方便在探索各项工具时持续迭代。

## 练习

| 课程 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建仓库和 Codespace |
| [1. 安装 Copilot CLI][ex1] | 安装 | 安装并验证 Copilot CLI |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分状态，然后合并第一个 PR |
| [3. 智能体模式：Plan 和 Autopilot][ex3] | 智能体模式 | 根据议题规划功能，使用 Autopilot 实现，再在 Interactive 模式下审查 |
| [4. 使用自定义指令引导 Copilot][ex4] | 上下文 | 查看并更新指令，再将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复检查 | 查看现有技能、修改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 在 CLI 中配置 MCP 并检查筛选行为 |
| [7. 创建并使用 QA 智能体][ex7] | 要求和覆盖率 | 创建并选择专用配置，收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查和合并 | 审查完整更改、创建 PR 并使用 Agent Merge |
| [9. GitHub Copilot CLI 斜杠命令][ex9] | CLI 功能 | 探索上下文、模型、共享及可选的云智能体委派 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、可复用自定义项和更多资源 |
| [可选：集成 Foundry][foundry] | 托管智能体 | 准备模型、部署以目录为依据的智能体并将其连接到网站 |

## 先决条件

参加本工作坊前，请确保已具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 对终端/命令行操作有基本了解
- [ ] 已安装并配置 Git
- [ ] 有权创建仓库和 Codespace

> [!TIP]
> 没有付费计划？已验证学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊所用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完整完成所有路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 如果使用的是 Copilot Business 或 Copilot Enterprise，请确认管理员已启用 Copilot CLI。

## 开始

**[从先决条件开始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
