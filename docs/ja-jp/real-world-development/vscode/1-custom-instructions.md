---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法の希望や背景情報を Copilot が参照できるようにするには、[指示ファイル][instruction-files]を使います。この演習では、リポジトリ全体とパスごとのカスタム指示を確認し、指示の変更がコードに与える影響を試します。

この演習では、次のことを行います。

- リポジトリ全体とファイル種別ごとの指示を確認します。
- 既存の指示で publishers helper を生成します。
- `.github/copilot-instructions.md` にプロジェクト共通の規則を追加します。
- 同じプロンプトを再実行し、生成コードが新しい規則を反映するか確認します。
- 次の演習で利用できるよう、指示と helper の変更を `main` にマージします。

> [!CAUTION]
> Copilot が生成するコードは、設定したすべての規約に必ず従うとは限りません。目的は、指示を更新した後の傾向の変化を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。指示ファイルを使い、Copilot が次の規約に沿って作業できるようにします。

- データ層には必ず単体テストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、モダンな見た目にします。
- コードに TypeScript Documentation (TSDoc) コメントを追加します。
- 各ファイルの先頭に、内容を説明するコメント ブロックを追加します。

### カスタム指示

カスタム指示は、コーディング規約やライブラリなどのコンテキストを Copilot に伝えます。リポジトリ全体にも、特定のファイル種別や作業にも適用できます。

- `.github/copilot-instructions.md` は、このリポジトリへのすべてのリクエストに送られる指示ファイルです。技術スタックやプロジェクトの概要など、全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、作業に関する指針を定義します。

リポジトリ共通のファイルには、技術スタック、開発中のものの概要、ベストプラクティスなど、Copilot Chat や CLI へのほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。個別の指示ファイルでは、TypeScript や Astro などの言語・ファイル種別、UI コンポーネントや単体テストの作成などの作業に関する指針を示します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ共通の指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使用します。
>
> Copilot は [ほかの規格の指示ファイル][custom-instructions-support]にも対応しています。たとえば AGENTS.md や CLAUDE.md などです。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは、指示ファイルの作成方法を詳しく扱いませんが、サンプル プロジェクトの例は一般的な方法を示しています。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクト構成、全体的なコーディング規約など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ層など、特定のファイル種別や作業の指針を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードのよい例・避けるべき例を示します。

指示ファイルの作成方法にも、AI の使い方と同じく唯一の正解はありません。プロジェクトに合う方法は、試しながら見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイル群を用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業の指示ファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は利用する環境によって異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。該当する操作は、それぞれのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどのリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

プロジェクトに同梱されている指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまな作業向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらの指示は、Copilot とのあらゆるやり取りに適用されます。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリのルートからの glob で適用対象を指定します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトの単体テストを作成するための具体的な指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参照する例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内ドキュメントの規則はまだありません。次の手順で、TSDoc コメントとファイルコメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ここでは変更を専用のブランチで管理します。

1. 前の演習で使った Codespace に戻ります。
2. 新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

カスタム指示の影響を確認するため、まず現在の指示がある状態でプロンプトを送ります。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で案内します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを見るようにします。
2. `src/lib/publishers.ts` を開き、helper の配置先を示します。
3. Chat ビューの agent picker で **Agent** を選択し、Copilot がファイルの変更を適用できるようにします。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新する様子を確認します。helper とテストの両方が変更されることがあります。
6. 提案された helper が型付き関数で、最初の引数として `db` クライアントを受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 生成されたコードに TSDoc コメントとファイル先頭のコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加する場合があります。その場合も、指示更新後に一貫性が高まるかを確認することが目的です。

## リポジトリ共通の規則を追加する

`.github/copilot-instructions.md` にコード内ドキュメントの規則を追加します。

1. ファイルを開き、**Code formatting requirements** セクションを探します。既存の規則にはコーディング標準が記載されていますが、コード内ドキュメントの規則はありません。そのため、helper にコメントがありませんでした。
2. 既存の規則の直後に、ファイルコメント ヘッダーと TSDoc コメントを追加する次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、指示ファイルはリポジトリ全体のガイダンス用の `.github/copilot-instructions.md` にも、特定の言語、ファイル種別、作業向けの `*.instructions.md` にも作成できます。ここで追加したドキュメント規則のような、プロジェクト全体の標準にはリポジトリ共通のファイルが適しています。

## 同じプロンプトを再実行して変化を確認する

更新した指示を Copilot に読み込ませ、同じ helper 作成を依頼します。

1. Copilot Chat で **New Chat** を選択します。
2. `src/lib/publishers.ts` を開き、Copilot が対象ファイルを把握できるようにします。
3. 前と同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のような目的を説明するコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディング プラットフォームの publisher データアクセス helper。
    * データベースから publisher 情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような目的、パラメーター、戻り値を説明する TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * すべての publisher の ID と名前を含む一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns publisher オブジェクトの配列に解決する Promise。
    */
   ```

Copilot に新しいプロジェクト標準に従わせ、次の演習で使う実際のコードに適用できました。

## 指示と変更をコミットしてプッシュする

指示ファイルと helper も、ほかの変更と同じようにソース管理で管理します。

1. 新しいターミナルで次を実行し、変更を確認します。

   ```bash
   git status
   ```

2. 指示ファイル、helper、および helper 用に Copilot が変更した関連ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにある指示ファイル更新の Issue と関連付けて PR を作成します。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せましょう。Issue を見つけ、PR を作成して、その Issue を完了する内容にします。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは初めてです。プロジェクト テンプレートには GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められる場合があります。案内に従って許可してください。この接続で使われる Model Context Protocol (MCP) の仕組みは、後の演習で説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 次のプロンプトを使い、指示ファイル更新の Issue を探して、現在のブランチから PR を作成し、指示の更新と今後のフィルター作業に向けた publishers helper の基盤の両方を説明します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認し、問題がなければ **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェル コマンドの許可を求めたら、内容を確認してから **Allow** を選択します。

GitHub Copilot を使って PR を作成し、マージできました。

## まとめと次のステップ

指示ファイルが Copilot のコンテキストに使われることを確認しました。この演習では、Copilot Chat を使って次のことを行いました。

- リポジトリ共通の指示とパスごとの指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント規約が Copilot に伝わる方法を確認しました。
- 既存の指示でコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にプロジェクト共通の規則を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい規則を取り入れる様子を確認しました。
- 指示の更新と helper の基盤をコミットし、`main` にマージしました。

次は指示を活用して[エージェントモードで機能を実装します][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [よりよいカスタム指示を作成する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
