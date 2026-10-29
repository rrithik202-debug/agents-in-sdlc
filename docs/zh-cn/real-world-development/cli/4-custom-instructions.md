---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看仓库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的指令、添加文档规范，并将其应用到代码中。

在本课中，你将：

- 了解仓库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践规范，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并记录注入的 `db` 参数（如有）。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

你将通过指令文件向 Copilot 提供这些规范，以便它按要求完成工作。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以为整个仓库或特定文件类型设置指令，例如首选编码规范、库以及注释风格。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件，应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

入门项目已包含一组指令文件。先查看现有文件，再进行更改，以了解指令产生的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹，查看其中的文件，包括 Astro、Drizzle 数据层和测试指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段；该字段指定指令适用的文件 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种方式便于拆分和复用指令，也能为 Copilot 提供参考示例。

## 更新指令以符合团队规范

现有文件是不错的起点，但还缺少一项规范。修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 在该部分末尾添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在启动对话时加载仓库指令。保存更新后，退出并恢复筛选对话，使 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。确认新增的 TSDoc 注释准确说明了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何读取指令文件，并将新规范应用于功能。具体来说，你：

- 了解仓库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件以确保遵循编码规范。
- 查看指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
