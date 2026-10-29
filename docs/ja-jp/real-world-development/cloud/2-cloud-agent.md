---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といった作業を自律的に実行します。完了すると、人間がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を委任することで開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys では、最初のサイトを作成した委託先がドキュメントを適切に残さず、コードにドキュメントがほとんどありません。まず、アプリケーションの公開関数すべてに TSDoc コメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを表示します。完成度の高い実装ではなく、ユーザー体験 (UX) の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、より優先度の高い Issue もあります。

どちらも優先度が下がりやすい作業であり、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業している間、開発者はほかの作業に取り組み、後から成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当て後、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップして作業を始めます。実行中でも完了後でもセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法があるわけではなく、考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、結果に合わせて進めます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot をガイドする方法を確認しました。まだ `.github/copilot-instructions.md` を見ていない場合は、ここで簡単に確認します。cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認しておくと、後でその影響を見つけやすくなります。

GitHub Web UI または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` の **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントの要件を見ます。
- `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、適切な状態を保つための環境やセットアップ スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を準備できます。このワークショップでは演習を進めやすくし、セットアップを体験できるよう、あらかじめ設定しています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー バイナリ、単一 Astro アプリ用に移行とシードを済ませた SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限だけを設定します。Copilot の操作には Copilot 独自のトークンが使われます。
    permissions:
      # 依存関係のインストールなどでセットアップ中にリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じ形式ですが、いくつか要点があります。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったくないことがあります。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に依頼するのに適した作業です。後の演習で、バックグラウンドで作成された内容をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの、新しく作成されたプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot がタスクリストを作成したことを確認します。

> [!NOTE]
> Copilot のタスクリストが PR に表示されるまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作り、プロジェクトを分析して、問題への最適な取り組み方を決めています。

13. タスクリストと、実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調査して現状を把握し、計画を考えて決定し、コードを作成する過程が表示されます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で実行し、開発者がほかのことに取り組めるようにすることです。この利点を活かし、別のタスクも Copilot cloud agent に割り当てている間に、アプリケーションの機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチームが求める関連ゲームのセクションにあまり時間がかからなくても、その時間を別の作業に使えます。cloud agent にこのタスクも割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが分かります。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、完了後にレビューすればよく、常に監視する必要はありません。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を Copilot に割り当てて非同期に作業させることができます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

この演習では、次のことを確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も使用します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
