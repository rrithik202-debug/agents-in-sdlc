---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作するか確認します。自分でアプリをクリックして確認する代わりに、Playwright MCP サーバーを接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを提供する方法を確認し、Playwright MCP をワークスペースに追加し、ブラウザーでフィルター機能をテストしてからブランチを公開します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーからフィルター機能を手動テストしてもらいます。
- ブランチを公開し、フィルター機能のプルリクエストを開きます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、エージェントモードはエディターの外部にある機能も利用できます。MCP は、エージェントが外部のツールやサービスと通信するための標準的な仕組みです。MCP により、AI エージェントはリソースを使って最新情報にアクセスし、ツールを使って操作を実行できます。

MCP サーバーは、AI エージェントと外部ツールの間の通信を管理する橋渡し役です。既存の API や NPM パッケージなどのローカルツールとエージェントをつなぎます。各 MCP サーバーは、それぞれ異なるツールとリソースを提供します。

- **[GitHub MCP Server][github-mcp-server]** は、GitHub リポジトリを管理するアプリケーション プログラミング インターフェイス (API) を提供します。AI エージェントでリポジトリの作成や更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server][playwright-mcp-server]** は、Playwright によるブラウザー自動化機能を提供します。AI エージェントで Web ページへの移動、フォームへの入力、ボタンのクリックなどを行えます。

さまざまなツールやリソースを提供する MCP サーバーがほかにもあります。GitHub の [MCP registry][mcp-registry] で見つけたり、コントリビュートしたりできます。

![エージェントモードとコンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

> [!CAUTION]
> MCP サーバーはほかの依存関係と同様に扱ってください。利用前にソースコードと公開者を確認し、セキュリティへの影響を検討します。信頼できるサーバーだけを使い、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` は、VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` サーバーを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この設定はプロジェクトのテンプレートに含まれています。前の演習で Copilot がバックログの Issue を読めたのは、このリモート GitHub MCP サーバーを使ったためです。ローカルにインストールする必要はなく、VS Code が HTTP で接続し、初回利用時に GitHub 認証を行います。

## Playwright MCP サーバーを追加する

Playwright MCP を追加すると、Copilot がブラウザーを操作して機能を確認できます。

1. `.vscode/mcp.json` の `servers` に `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`github` はリモートサーバーですが、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、デスクトップ画面がない codespace でブラウザーを表示せずに実行するための指定です。

> [!NOTE]
> Tailspin Toys はエンドツーエンドテストに Playwright を使うため、必要なブラウザーは通常インストール済みです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。初回にサーバーのツールが必要になると、構成を信頼するよう求められます。

1. `.vscode/mcp.json` の `playwright` エントリ上部に表示される **Start** を選択します。
2. サーバーを信頼する確認が表示されたら、構成を確認して信頼を選択します。

手動で起動する手順は任意です。省略した場合は、次の操作で Copilot がツールを必要としたときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

Playwright MCP サーバーを使うと、Copilot がブラウザーでページを開き、移動し、フィルターを適用し、結果を読み取って、確認した内容をまとめます。会話を離れずに機能を確認する簡単な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ラベル付きのボタン、リンク、リスト項目などの構造化された要素を判断するため、機能確認と簡単なアクセシビリティ確認を同時に行えます。

アプリが起動している状態で、Copilot に次のプロンプトを送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot はブラウザーを起動して各手順を実行し、結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。問題があれば、PR を作成する前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テストには `http://localhost:4321` で起動したアプリが必要です。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。初回利用時にブラウザーのダウンロードが必要な場合があります。見つからないと報告されたら `npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能を確認したので、チームのレビュー用にブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot の変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot が変更したファイルを表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

GitHub.com や GitHub コマンドライン インターフェイス (CLI) などで PR を作成する方法もありますが、GitHub Copilot に関連する Issue を探して PR を作成してもらいます。PR は Issue に関連付けられます。

1. Copilot Chat を開き、**New Chat** を選択します。
2. agent picker で **Agent** が選択されていることを確認します。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を取得したり操作したりする際に **Continue** を選択します。GitHub ツールの初回利用時は認証を求められることがあります。
5. Copilot が Issue を探し、適切な Issue を見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを開いて PR をレビューします。まだマージしないでください。

## まとめと次のステップ

おめでとうございます。この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に、ブラウザーからフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを開きました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティの確認など、特定の作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP が注目されている理由][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
