---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意の cloud agent への委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI には、操作や高度な機能、設定を管理するスラッシュコマンドが多数あります。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。このレッスンでは、ほかの便利なコマンドを確認します。

このレッスンでは、`/context` と `/compact` で会話のコンテキストを確認し、`/model` で利用可能なモデルを調べ、`/share` でセッションをエクスポートまたは共有します。また、並列作業、worktree、cloud agent への委任に使える任意のコマンドも確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えたので、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュコマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示・ツール・メッセージ・空き領域ごとのコンテキストの内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比べます。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に内容を圧縮します。タイミングを選びたい場合は `/compact` を使います。関係のない別のタスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野が異なります。Copilot CLI では、利用可能なモデルの一覧を表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの使い方を全員で改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンに表示される `/share` の選択肢を確認します。
2. 共有する場合は、`/share file` によるローカルの Markdown エクスポートなど、目的に合った方法を選びます。
3. 送信または公開する前に、エクスポートした内容を確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を予定していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並列化する、または委任する

基本的なワークショップはここで完了です。Copilot CLI には大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクに分け、複数のサブエージェントで並行実行します。
- `/worktree` は別の作業用に分離された Git worktree を作成します。
- `/delegate` はタスクを Copilot cloud agent に送信します。非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドは追加の worktree やリモート作業を作る可能性があるため、任意です。試す場合は新しい会話で範囲の明確なタスクを始め、通常のワークフローで結果をレビューします。非同期エージェントの作業を詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って、CLI を設定し、セッションを共有し、内部の動作を確認しました。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

利用できるスラッシュコマンドはほかにもあり、Copilot CLI で試せることはまだたくさんあります。次は[これまでの内容と、さらに学ぶための次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
