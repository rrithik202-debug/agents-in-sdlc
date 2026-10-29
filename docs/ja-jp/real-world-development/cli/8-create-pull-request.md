---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルター機能、指示、スキル、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能、指示の更新、スキルの変更、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成します。最初の PR は手動でマージしました。今回は **Agent Merge** に進行を任せます。

> [!NOTE]
> 通常は機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは学習を簡潔にするため、フィルター機能と品質ワークフローを 1 つの会話とブランチにまとめています。

このレッスンでは、Agent Merge の仕組みを確認し、変更と検証結果をレビューして、フィルター機能の PR を作成します。レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、残りの PR 作業を自動化しながら、マージを許可する権限は開発者が保持したいと考えています。

## Agent Merge について

**Agent Merge** は、PR をマージするまでに必要な作業を自動化します。有効にすると、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要なリベースなど、PR をブロックしている作業に Copilot が取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge を有効にする前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードを作成できました。変更全体を一緒にレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` を入力して Agent Merge を有効にします。
6. Copilot が PR の CI を監視し、すべて成功した後にマージすることを確認します。
7. `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
