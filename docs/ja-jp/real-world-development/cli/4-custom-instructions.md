---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント規約を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業の進め方に希望がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、このレッスンでは Copilot が使った指示を確認し、ドキュメント規約を追加してコードに適用します。

このレッスンでは、リポジトリとパスごとの指示ファイルを確認し、コーディング規約を追加して、コードに対する影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントでは、コードをそのまま言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の公開関数は、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。該当する場合は、差し替え可能な `db` 引数も文書化します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたときにコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルなどのコンテキストや希望を Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。好みのコーディング規約、ライブラリ、コードに含めたいコメントなどを指定できます。リポジトリ全体にも、特定のファイル種別にも指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリのすべてのリクエストに送られる指示ファイルです。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、作業に関する指針を定義します。

前者にはほとんどのリクエストに関係するプロジェクトレベルの情報を、後者には個別の対象に関する指針を記載します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を前提にする前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

まず、スターター プロジェクトに用意された指示ファイルを確認してから変更を加え、その影響を見ます。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング指針を確認します。これらの指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` が対象ファイルを決める glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` のリンクから、再利用可能な指示ファイルやプロジェクト内の例を確認します。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、足りない部分があります。新しく生成する TypeScript に TSDoc コメントを追加するため、中心となる `copilot-instructions.md` を更新します。

1. **Code formatting guidance** セクション（35 行目付近）を探します。
2. 最後の箇条書きに次の Markdown を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター機能のコンテキストを保ったまま新しい指示を読み込むため、会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力します。会話は自動的に保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態で、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に指針の適用を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを得る方法を確認し、新しい規約をフィルター機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスごとの指示ファイルがエージェントに読み込まれる方法を確認しました。
- コーディング規約に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルを調整して実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示への対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
