---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここ数回の演習では、ユーザー体験を改善して機能を追加する 3 つの作業を Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。GitHub.com の[エージェントページ][agents-page]では、開いているプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、エージェントページで cloud agent のタスクを監視し、進行中のセッションに追加機能を依頼します。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと気付きました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。ハイコントラストに加えてライトモードも実装するよう、エージェントに追加の指示を送ります。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域に表示されるタスク一覧を確認します。Copilot に割り当てた次のような作業が表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更するアプリケーション プログラミング インターフェイス (API) を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードを取得する。
   - Copilot が作業する環境を作成する。
   - Model Context Protocol (MCP) サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR パネルも確認します。PR と変更されたファイルを確認して、作業を詳しく監視できます。

## cloud agent に追加の指示を送る

現在実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関連するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

   ![Copilot agents ページのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。作業開始後は、次の例のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents ページにあるタスクと、強調表示された「Steer active session while Copilot is working」ダイアログのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に含めたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わった後に、追加の要件を作業に反映します。

今回も完了まで数分かかります。待ち時間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視し、方向付けました。このページでは、進行中と完了済みのタスクを追跡し、実行中の作業を確認して、セッションの範囲を変更したり追加の指示を送ったりできます。

このレッスンでは、次のことを行いました。

- Copilot agents ページを管理画面として使用し、cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[作成されたプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
