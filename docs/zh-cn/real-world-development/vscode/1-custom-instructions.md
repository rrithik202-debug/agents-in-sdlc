---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是[指令文件][instruction-files]，它们可为代码生成提供指导。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在保留**当前**指令的情况下生成筛选功能的第一项数据层更改（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的仓库级标准；
- 重新运行相同提示，观察生成的代码如何遵循新标准；
- 将指令更新和筛选功能基础更改提交到 `main`，供 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分设定的标准不完全一致。Copilot 的结果具有非确定性。本练习旨在观察更新指令后行为的**趋势**变化，而不是要求输出与示例逐字相同。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践规范和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TypeScript 文档注释（TSDoc）为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。它们能帮助你引导 Copilot 提供更相关的建议和代码片段。你可以为整个仓库或特定文件类型设置指令，为相应任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。它应包含项目级信息，即与大多数 Copilot 请求相关的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，用于提供特定语言（如 TypeScript 或 Astro）或特定任务（如创建 UI 组件或一组新单元测试）的指导。

> [!NOTE]
> 在集成开发环境（IDE）中使用时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用仓库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [也支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

关于如何创建指令文件，完整讨论超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

指令文件没有唯一的编写方式，AI 的使用方式也不止一种。通过实践，你会发现最适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目中的指令文件时，你会发现其中包括针对[用户界面（UI）更新][ui-instructions]和 [Astro][astro-instructions] 等任务的文件。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前工作环境的相关课程会在适用时说明。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义指令文件

花些时间阅读此仓库中提供的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设定一个相对于仓库根目录的 glob，用于确定指令适用的文件。此处，匹配 `**/*.test.ts` 等模式的 TypeScript 测试文件都适用。
5. 查看为本项目创建单元测试提供的具体指导。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供可遵循的代码示例。（其中的路径相对于指令文件，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循良好实践，为更改创建一个分支。

1. 从上一练习返回 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用模式和模型可能不同；需要时会在相应步骤中说明。

要查看自定义指令的影响，先在当前指令文件保持不变的情况下发送提示。稍后更新指令后，再发送相同提示进行比较。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，建议的辅助函数是一个类型化函数，它以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这遵循 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此即使没有明确要求，它也可能添加文档注释。如果出现这种情况也没关系；本练习的重点是观察更新指令后行为的一致性改进。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。注意，该部分记录了项目编码标准，但尚无代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在仓库级别（`.github/copilot-instructions.md`）创建全局指导，也可使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。仓库级文件适合放置刚才添加的文档注释规则等项目范围的标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在建议的文件开头会包含类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，现在建议的函数会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到实际代码中；下一练习将在此代码基础上继续。

## 提交指令并推送分支

指令文件与仓库中的其他资源一样，使用常规源代码管理流程进行管理。现在提交指令和推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，应创建拉取请求，并关联待办事项中的文档标准议题。也可以手动完成，但 Copilot 可以使用已连接到项目的 GitHub 工具代为处理。请提示 Copilot 查找议题、创建用于关闭该议题的拉取请求（PR），然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 请求 **Allow** 以代表你执行 GitHub 操作时，审查操作并酌情选择 **Allow**。
6. 创建 PR 后，使用以下提示请 Copilot 合并 PR 并将当前分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求 **Allow** 以代表你执行 GitHub 操作和 shell 命令时，审查操作并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的指令文件，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 使用**现有**指令生成的结果；
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准；
- 重新运行相同提示，观察建议的代码如何遵循新标准；
- 将指令更新和辅助函数基础更改提交到 `main`，供下一练习使用。

接下来，Copilot 会使用这些指令为整个代码库添加新功能。继续学习[智能体模式][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
