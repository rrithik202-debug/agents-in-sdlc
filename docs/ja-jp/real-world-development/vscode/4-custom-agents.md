---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` に Markdown ファイルを置き、対象を絞った指示、ベスト プラクティス、コード パターン、分野固有の知識を定義すると、Copilot がその作業をより効果的に行えるようになります。チームの専門知識を再利用可能なエージェントにできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] に沿うアクセシビリティエージェント、安全なコーディング規約に従うセキュリティエージェント、テスト パターンの一貫性を保つエージェントを作成できます。

プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` に Markdown ファイルを置いてカスタムエージェントを定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。分かりやすく言えば、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。この演習では、アクセシビリティのカスタムエージェントがガイドラインに沿ってサイトを確認・更新します。その際、プルリクエストのワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェント作成に唯一の正解はありません。実際に試し、環境やシナリオに合わせて調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もが使いやすいクラウドファンディング サイトを目指しています。最近、現在のダークテーマはテキストと背景色のコントラストが不十分で読みにくいという利用者の意見がありました。このアクセシビリティ上の課題に対応するため、デザインチームは利用者が切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティエージェントを確認する

作業を依頼する前に、エージェントの指示を確認します。

1. Codespace に戻り、新しいブランチを `main` から作成します。フィルター PR とは分けて管理します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat では、`.github/agents` に定義されたカスタムエージェントが Chat ビュー下部の agent picker に表示されます。エージェントを選ぶと、そのエージェントの指示とツールに範囲を絞ったチャット セッションを開始できます。

1. Copilot Chat を開き、**New Chat** を選択します。
2. agent picker で **Agent** を選択します。
3. Chat ビュー下部の agent picker を開き、**Accessibility agent** を選択します。
4. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調査して変更を提案する様子を確認します。
6. **Files changed** の表示で、エージェントが変更したファイルを確認します。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどを変更します。変更内容は次の演習で確認します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野向けの専門的な AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の種類の作業をより効果的に行えるようにできます。

この演習では、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェントモードでカスタムエージェントを使いました。

次は[エージェントの作業を監視して方向付けます][next-lesson]。進行中の変更を確認し、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [Awesome Copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前の演習: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次の演習: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
