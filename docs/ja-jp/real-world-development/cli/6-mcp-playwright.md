---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードの作成だけでなく、データや外部サービスとの連携、追加の自動化も必要です。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスも利用できるようになります。

このレッスンでは、Model Context Protocol (MCP) と Copilot CLI での使い方を確認し、必要に応じて Playwright MCP サーバーを追加して、エージェントにブラウザーでフィルター機能を確認してもらいます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作してフィルター機能を確認してもらいます。

## シナリオ

単体テストとエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。利用者と同じようにサイトを操作する機能を Copilot に提供し、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは外部ツールやサービスとリアルタイムで通信し、リソースを使って最新情報にアクセスしたり、ツールを使って操作を実行したりできます。

これらのツールやリソースは、エージェントと外部ツールやサービスの橋渡しをする MCP サーバーを通じて利用します。各 MCP サーバーは、エージェントがアクセスできる異なるツールとリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]** は、GitHub リポジトリ、Issue、プルリクエストを管理する API を提供します。
- **[Playwright MCP Server][playwright-mcp-server]** は、Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub の [MCP registry][mcp-registry] で検索やコントリビューションができます。

> [!CAUTION]
> MCP サーバーもほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、公開者を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP がまだ Copilot CLI で利用できない場合は追加します。

> [!NOTE]
> Playwright MCP がすでに設定されている場合は、この手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を入力します。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) で保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

先ほどは手動で機能を確認しました。今度は Copilot CLI で利用できる Playwright MCP サーバーに同じ確認を依頼します。

1. 次のプロンプトを使い、Copilot に機能の確認を依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、適切な MCP サーバーを自動的に判断するため、明示的に指定しなくてもかまいません。使うサーバーが分かっている場合はプロンプトで示すと、より一貫した結果につながり、トークンも少し節約できます。

2. Copilot がブラウザー操作で機能を確認する手順を見ます。
3. 結果を読み、期待どおり動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、最後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使って、Copilot CLI から実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- まだ設定されていない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作してフィルター機能を確認してもらいました。

次はスキルとブラウザーツールをまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP が注目されている理由][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
