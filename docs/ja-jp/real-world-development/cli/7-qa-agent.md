---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーのフィルター動作を確認しました。ここでは、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、カスタムエージェントと指示・スキル・MCP ツールの連携を確認し、再利用可能な品質保証 (QA) プロファイルを作成して、その結果をフィルター機能の Issue と照らし合わせます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な QA プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) の前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントで QA の流れを調整し、再利用可能なレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、目的、指示、利用可能なツールを Markdown プロファイルで定義する、専門化された Copilot です。この演習では `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

それぞれのカスタマイズには異なる役割があります。リポジトリの指示はチームの規約を示し、`quality-checks` スキルは再利用可能なチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルはこれらを使って要件を評価し、結果を報告するよう Copilot に指示します。既存の機能を置き換えるものではなく、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

PR を作成する前に、QA のチェック内容と作業範囲を定義したプロファイルを用意します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロファイルを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

生成したプロファイルが目的と制約を正しく反映していることを確認してから使用します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須で、`name` は任意ですが、含めると表示名が明確になります。
3. プロファイルの指示を読み、要件の確認、リポジトリ指示の遵守、`quality-checks` スキルの実行、Playwright MCP の利用が含まれていることを確認します。
4. 証拠を示した報告、実装コードを変更する前の確認、コミットと PR を行わない制約が含まれていることを確認します。
5. 不足や誤りがあれば、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後にフィルター機能の会話を再開し、QA を選ぶと、Issue と計画のコンテキストを引き継げます。

1. 次のコマンドを入力してエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しく作成したエージェントは、利用可能なエージェント一覧にまだ表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始することを確認します。
4. 作業が終わったら、QA エージェントの報告を読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに加え、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な QA プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue に対する調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は結果をまとめ、[機能 PR を作成して Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
