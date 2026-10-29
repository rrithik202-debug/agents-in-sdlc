---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、環境を準備します。Tailspin Toys リポジトリのコピーと codespace を作成し、指示ファイルを編集して cloud agent の成果を確認できるようにします。

## ワークショップ用リポジトリをセットアップする

テンプレートからリポジトリを作成します。新しいリポジトリには、演習に必要なファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. リポジトリの **Use this template** ボタンを選択し、**Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリのテンプレート設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。以降の演習で使用します。

## codespace を作成する

指示ファイルを編集し、cloud agent の作業を確認するために codespace を使います。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、フル機能の統合開発環境 (IDE) を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** を選択して新しい Codespace を作成します。

   ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手作業でインストールするよりもはるかに短時間で済みます。作成を待つ間に、これから使う GitHub Copilot のほかの機能を確認してもかまいません。

> [!CAUTION]
> 今後の演習でこの codespace に戻ります。ブラウザーのタブを閉じずに残しておいてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。ローカルで行う場合は、VS Code でクローンしたリポジトリを開き、求められたら **Reopen in Container** を選択します。codespace と同じ開発コンテナーが作成されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ワークショップ用リポジトリのコピーを作成し、cloud agent と並行して作業するための codespace の作成を始めました。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1: カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
