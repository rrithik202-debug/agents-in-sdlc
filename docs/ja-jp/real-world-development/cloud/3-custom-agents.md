---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` に Markdown ファイルを置くことで、Copilot に対象を絞った指示、ベスト プラクティス、コード パターン、分野固有の知識を与え、その種の作業を効果的に進められます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] に沿うアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが含まれます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には一部重なるところがあります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業を行う担当者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整します。この演習では、アクセシビリティ用カスタムエージェントがサイトをアクセシビリティ ガイドラインに沿って確認、更新します。その際、プルリクエストのワークフローやテスト実行などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI を使うほかの作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使いやすいクラウドファンディング プラットフォームを目指しています。現在のダークテーマはテキストと背景のコントラストが不十分で、読みにくいというフィードバックが寄せられています。デザインチームは、利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、この機能をすばやく実装したいと考えています。カスタムエージェントに機能を生成させます。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。Copilot をどのようにガイドするか、ファイルの内容を確認します。

1. codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。
2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` のフィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

4. 続くセクションを読み、アクセシブルな Web サイトのコード生成に関する中核的な責任、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴を確認します。

## Issue を作成して割り当てる

Copilot agents ページは、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を送れます。ここではハイコントラストモードの作成を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で、カスタムエージェントと Accessibility が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に合わせて作成する専門的な AI アシスタントです。チームの専門知識と規約を、Copilot の作業をガイドする再利用可能なエージェントとしてまとめられます。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て。

Copilot がハイコントラストモードの実装を進めている間に、次は Copilot agents ページから[エージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
