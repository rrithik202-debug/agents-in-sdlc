---
title: "レッスン 5 - quality-checks スキルのカスタマイズと使用"
description: "既存の quality-checks スキルを調べ、報告形式を変更して、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。手動でコードを検証し、指示ファイルで規約に従わせましたが、テストや lint、継続的インテグレーション (CI) のほかの要素はどうでしょうか。

このような作業には**エージェントスキル**が適しています。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、既存の `quality-checks` スキルを確認し、結果の書式をカスタマイズして、スキルを再読み込みして実行します。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む再利用可能な作業手順、実行可能なスクリプト、補助リソースをまとめます。基本的には、スキルの名前を持つフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングの指針が含まれます。スキルによっては、スクリプトやリソース用のサブフォルダーもあります。

> [!NOTE]
> サブフォルダーや追加ファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルはプロジェクトにあるコマンドを使うため、`SKILL.md` のみで構成されています。

スキルはチームで共有できるプロジェクトの `.github/skills` に置くことも、個人用の `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックのために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって、Copilot がスキルを使うタイミングを判断しやすくなります。
3. 指示を読み、テストと lint の手順を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI にスキルの 3 つのチェックを実行するよう依頼します。

1. フィルター機能の会話に Interactive モードで戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行し、成功・失敗・ブロックのどれだったか、ツールが報告した内容が分かるようにスキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、変更したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 新しいレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックに使う `quality-checks` スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルター機能の PR に含めます。次は [Playwright MCP サーバーでサイトを操作し、Copilot が機能を確認できるようにします][next-lesson]。

## スキルの例

コミュニティの例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
