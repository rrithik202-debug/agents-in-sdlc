---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このワークショップでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot といくつかの作業に取り組みました。コードへのドキュメント追加、デザインチームが検討する関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

これまでに何度も確認したとおり、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、人の監視なしで作業するため、安全性を保つ制約が設けられています。

- Copilot はリポジトリの読み取り権限を持ち、書き込み権限はコードの作業に使うブランチ**のみ**にあります。
- cloud agent は GitHub Actions 内に作成される一時的な別環境で実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent がコードにドキュメントを追加した最初のプルリクエスト (PR) を、GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は作業の完了を待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントや、その他のドキュメントを含むコードを確認します。具体的な変更内容は実行ごとに異なります。

   変更を確認するときは、TSDoc コメント、TypeScript の規約、コメントヘッダーに注目します。これらは演習 2 の最初に確認したカスタム指示ファイルから反映されています。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR で作業するとき、コメントやコードへのインラインコメントでチームメンバーと同じように Copilot をメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ずしも決まった文面で依頼できるわけではありません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上の例などを参考に、コードベースに追加したいドキュメントの変更を **@copilot** にメンションしてコメントで依頼します。ほかのユーザーをメンションするときと同じように操作します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しいコミットが追加されたことを確認します。
5. **Files changed** タブで変更をレビューします。

納得できるまで反復してかまいません。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するために Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。
9. **任意:** Copilot が作成したブランチを Codespace でチェックアウトし、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。先ほどと同じように、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの件数を変えたり、コメントヘッダーや TSDoc コメントを追加したりできます（この Issue はカスタム指示を変更する**前**に割り当てたことを思い出してください）。変更を依頼するには、**Conversation** タブに新しいコメントを追加します。Copilot はコメントを確認して、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で Copilot agents ページから追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が付きます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装をレビューするときは、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - 利用者の設定を保持する local storage の使用
   - ハイコントラストモードとライトモードに関する CSS やスタイルの変更
   - ARIA ラベルやキーボード操作などのアクセシビリティ属性
   - モード切り替えを管理する JavaScript / TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。
10. **任意:** Copilot が作成したブランチを Codespace でチェックアウトして、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントは、アクセシビリティのベスト プラクティスに沿った機能実装を Copilot に促します。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent は、実際のバックログ項目を渡して別の作業に取り組めるときに力を発揮します。リポジトリに Issue を追加して Copilot に割り当てる練習をしましょう。たとえば、次の作業が考えられます。

- ゲーム詳細ページに支援者向けの関心表明フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認しました。** cloud agent の出力に後でどのように影響するかを確認しました。
- **Issue を Copilot cloud agent に割り当てました。** 環境を準備し、計画を立て、非同期に作業する様子を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使いました。** ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを管理画面として使用しました。** アクセシビリティ セッションを監視して作業中に方向付けました。
- **cloud agent のプルリクエストをレビューして反復改善しました。** **@copilot** に変更を依頼し、ワークフローの実行を承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。続けて Copilot を使う場合は、ほかのハーネスで別の環境を使ったシナリオを体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE を離れずに Copilot Chat のエージェントモードと MCP を使います。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次の作業を試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
