---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリの変更でも、通常は複数のファイルを更新し、テストなどの操作を行う必要があります。開発者は必要なファイルを探し、変更し、テストを実行し、デバッグし、更新し忘れたファイルを見つけて、さらに修正するという作業を繰り返します。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同じように、既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> エージェントモードと cloud agent は異なる体験です。エージェントモードは IDE 内で動作し、短いフィードバックサイクルで作業します。cloud agent は GitHub Issue やプルリクエストを通じて非同期にチームの一員のように作業します。

この演習では、エージェントモードでプロジェクトを調べて複数ファイルを変更し、ユーザーインターフェイス (UI) とデータ層にフィルター機能を実装し、生成された変更とテストをレビューします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて、連携した変更を行う方法。
- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームが増えてきたため、カテゴリと発行者で絞り込めるようにします。前の演習で publishers helper を追加しました。残りのデータ層、UI、テストを Copilot エージェントモードで完成させます。

## Tailspin Toys の Web サイトを起動する

変更前に現在のサイトを確認します。Tailspin Toys はボードゲームのクラウドファンディングサイトです。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。

再利用可能なデータアクセス helper は `src/lib/` にあり、インタラクティブな機能は標準の DOM API を使う小さな Astro `<script>` で追加します。

1. 前の演習後に `main` に戻っていることを確認し、Codespace に戻ります。
2. 新しいターミナルを開き、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. 用意されている `npm run dev` タスクで、単一の Astro 開発サーバーを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか確認するダイアログが表示された場合は、**x** を選択して閉じます。

4. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーの `http://localhost:4321` を選択します。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストする Codespace へのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、その機能を確認します。主な機能は次のとおりです。

- **ホームページ:** ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ:** ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で案内します。

Web サイトの初期状態は動作しますが、新しい機能を追加して改善します。テンプレートからプロジェクトを設定すると、バックログの Issue が自動的に作成されています。GitHub Copilot に項目を表示してもらいましょう。

1. Chat ビューの agent picker で **Agent** を選択します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. モデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、作業を進めやすくすることを意図しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、手順に書かれた内容と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを送信して、リポジトリの Issue 一覧と優先順位を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Issue を一覧表示するコマンドを実行するよう求められたら、**Continue** を選択します。
5. Copilot が作成した一覧を確認します。

Copilot は利用者にとって役立つ順に Issue を優先順位付けしています。

## 指示ファイルを確認する

コードを生成する前に、Copilot にコンテキストを与える UI 指示を確認します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイル種別ごとの指示ファイルへのリンク。たとえば Astro、Tailwind CSS、Drizzle などです。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示の再利用、大きな指示の分割、コード生成時に従う例やテンプレートの提示に役立ちます。

## フィルター機能を実装する

フィルター機能にはデータ層、テスト、ゲーム一覧ページの UI が関係します。Copilot に必要なコンテキストを渡し、実装を依頼します。

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページを更新して、フィルター UI を追加します。

また、コードベースにマージする前にテストを実行し、すべて成功することを確認する必要があります。Copilot エージェントモードにこれらの作業を依頼します。

1. 現在の会話を続けるか、Copilot Chat で **New Chat** を選びます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. agent picker で **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)
4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がデータ層、UI、テストを調べて変更する様子を確認します。

   Copilot は関連ファイルを探し、データ層 helper、UI、テストを見つけてから、ファイルを変更しテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テスト実行などを行います。複雑さやコードベースによって数分かかることがあります。作業中にエディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。
7. テスト実行を求められたら **Continue** を選択します。
8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで作業を続けます。

   ![Copilot エージェントモードの完了したチャット セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. すべて問題なく動作し、コードのレビューも終わったら、Copilot Chat で **Keep** を選択します。

> [!NOTE]
> Copilot は変更やテスト実行の途中で停止したり、テストが失敗したりする場合があります。生成とテストを繰り返してエラーがないと判断するまで、作業を続けることがあります。実装は必ずレビューしてください。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードで Tailspin Toys に新機能を追加しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて連携した変更を行う方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし、PR を作成します][next-lesson]。

### ボーナス演習: ページネーションを実装する

ゲーム一覧が増えた場合に備え、ページネーションの追加を試します。この演習で学んだことを使い、既存のベスト プラクティスと指示ファイルに従うよう Copilot に依頼します。ページサイズを利用者が選べるようにするか固定するかを決め、望む実装に必要なガイダンスをプロンプトに含めます。

- 既存のベスト プラクティスに従い、指示ファイルを使います。
- ページサイズを利用者が選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更やコンテキストを追加で伝えながら反復します。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モードの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
