---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue のフィルタリング機能を実装して、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しましたが、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業し、適切なものを適切な方法で構築できるようにします。このレッスンは、エージェントを使った一般的な開発プロセスを実践する一連のレッスンの最初です。Issue から新機能を計画し、コードが有効で機能が期待どおりに動作することを確認し、最終的にプロジェクトへ正しくマージします。

> [!NOTE]
> この機能の作業では、同じ会話とブランチを続けて使います。実際には種類の異なる変更を別々のブランチや PR に分けることもありますが、このワークショップでは主要な概念に集中するため、作業をまとめます。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この後、リポジトリの指示を更新し、既存の `quality-checks` スキルを調整し、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、利用者はカテゴリや発行者でゲームを絞り込む必要があります。バックログの Issue には要件がありますが、カテゴリを組み合わせる方法など、コーディング前に合意する詳細があります。Plan モードで判断を整理してから、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発に取り入れても、基本の流れは変わりません。むしろ、基本がいっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を記述した Issue から始めます。
2. 実装計画を作成します。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> チームや組織によって細かな点は異なりますが、ほとんどの開発フローは上記の流れに沿っています。

この標準的な進め方に従えば、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ審査を受けることを確認できます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive:** エージェントが変更を提案し、続行する前に開発者の入力を待ちます。
- **Plan:** エージェントが計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot:** エージェントが自律的にコードを書き、テストを実行し、作業が完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業に取りかかる前に、Codespace とリポジトリを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して作業ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` などが表示されます。`#` を使うと、GitHub の Issue や PR をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能を実装したりタスクを実行したりする方針を、Copilot に依頼する前に決められます。複雑な作業では、少し時間をかけて計画することをお勧めします。Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されることを確認します。
2. 追加済みの Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. Copilot がフィルターの動作について質問した場合は、希望に基づいて回答します。回答に唯一の正解はありません。
4. 計画ができたら <kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の判断と一致しない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできたので、次は実装します。Autopilot モードで Copilot に作業を任せます。

Autopilot では、Copilot が問題を解決できたと判断するまで繰り返し作業します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示される同様の選択肢を選びます。
2. モード表示が **Autopilot** になったことを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了とみなす前に問題を解決します。

## 実装をレビューして検証する

生成されたコードも、ほかのコードと同じようにマージ前にレビューする必要があります。コードを確認し、サイトを起動して、すべて問題なく動作するか確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに戻り、モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画で決めた内容に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードは問題なさそうです。では、動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行者のフィルター、およびその組み合わせを試します。
4. リセットと結果が空の場合の動作が Issue と決定内容に合うことを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、コード生成がドキュメント化された開発規約に従うように、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
