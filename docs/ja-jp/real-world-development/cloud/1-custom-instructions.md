---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の進め方に希望がある場合や、Copilot に知っておいてほしい背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリ全体のカスタム指示と、パスを指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ共通の規則を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では同じプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムで並べて影響を確認するのが難しいからです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。指示ファイルを使い、Copilot が次の開発手法に沿って作業できるようにします。

- データ層には常に単体テストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、モダンな見た目にします。
- コードに TypeScript Documentation (TSDoc) コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストと希望を Copilot に伝えます。好みの規約、ライブラリ、コードに付けたいコメントなどを指定できます。リポジトリ全体にも、特定のファイル種別にも指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストに送られる単一の指示ファイルです。技術スタック、作成しているものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に指針を与えるために作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ共通の指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。基本的には、`copilot-instructions.md` には作成中のもの、プロジェクト構成、全体に適用するコーディング規約など、プロジェクトレベルの指針を記載します。`*.instructions.md` には、単体テスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクに関する指針を記載します。自然な言葉で、コードのよい例と悪い例を含めて明確に説明します。

指示ファイルの作成方法も AI の使い方も、唯一の正解はありません。プロジェクトに合った方法を、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。操作方法は各製品で異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。該当する操作は、それぞれのハーネスのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読み、Copilot が参照する情報を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドがリポジトリのルートからの glob を定義し、適用対象を決めます。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルと、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot が参考にできる例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する規則はまだありません。次に TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ共通の規則を追加する

この演習で行う編集は、コード内に TSDoc コメントとファイルレベルのコメントヘッダーを置く規則の追加です。後の演習で Issue を割り当てるとき、cloud agent がこの規則を参照します。先に作業用ブランチを用意します。

1. codespace のターミナルでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を伝えるためのファイルです。コーディング規約を明記し、コード提案の改善に役立てます。

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクション（27 行目付近）を探します。現在はコード内のドキュメント規則がなく、helper にコメントが生成されなかった理由を確認します。
3. 既存の規則のすぐ後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイル種別、タスクに固有の指示は `*.instructions.md` に置けます。今回追加するコメントの規則は、プロジェクト全体に適用するためリポジトリ共通のファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読みます。次の演習では Copilot が `main` からブランチを作成するため、cloud agent が指示を使えるように、変更を `main` に取り込みます。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に取り組むとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、変更を `main` にコミットできます。ここでは実際のプロジェクトに近い流れにするため、ブランチを使います。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ共通の規則を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストをレビューし、規則が適用されていることを確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [カスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
