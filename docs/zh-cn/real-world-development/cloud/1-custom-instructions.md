---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]可提供此类指导，帮助 Copilot 不仅了解你希望它做什么，也了解你希望它如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的仓库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）工作环境不同，这里不会运行“前后对比”提示：Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到指令文件发挥的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代感。
- 应使用 TypeScript 文档注释（TSDoc）为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获取正确的信息，并按这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。它们能帮助你引导 Copilot 提供更相关的建议和代码片段。你可以为整个仓库或特定文件类型设置指令，为相应任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个指令文件。它应包含项目级信息，即与大多数 Copilot Chat 或 CLI 请求相关的上下文，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的文件，用于提供特定语言（如 TypeScript 或 Astro）或特定任务（如创建用户界面（UI）组件或一组新单元测试）的指导。

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

## 添加新的仓库标准

接下来只需在这里编辑一次：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释，并为每个文件添加注释头。稍后云智能体处理你分配的议题时，会读取这条规则。

编辑前先创建一个分支（云智能体会读取议题目标分支中的指令，但你仍会单独提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体处理下一练习中分配的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过分支步骤并直接提交到 `main`。这里创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的指令文件，并在 `.github/copilot-instructions.md` 中添加了一条新的仓库级标准。之后审查云智能体创建的拉取请求时，可以看到这条标准发挥的作用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
