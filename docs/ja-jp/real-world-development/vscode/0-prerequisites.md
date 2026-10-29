---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、環境を準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ワークショップ用リポジトリをセットアップする

テンプレートからリポジトリを作成します。新しいリポジトリには、演習に必要なファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリのテンプレート設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。以降の演習で使います。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこれらの Issue に取り組みます。自分で Issue を作成する必要はありません。

## codespace を作成する

次に、演習で使う codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、フル機能の IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** を選択して Codespace を作成します。

   ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、必要なサービスを手作業でインストールするよりもはるかに短時間で済みます。作成を待つ間に、これから使う GitHub Copilot のほかの機能を確認してもかまいません。

> [!CAUTION]
> 後の演習でこの codespace に戻ります。ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。ローカルで行う場合は、VS Code でクローンしたリポジトリを開き、求められたら **Reopen in Container** を選びます。codespace と同じ開発コンテナーが構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

VS Code でエージェントモードを使うには、統合開発環境 (IDE) に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespaces では、通常すでにインストールされています。

> [!TIP]
> 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat に `Hello world` などのメッセージを入力して送信します。
4. 認証を求められた場合は、案内に従って GitHub アカウントにサインインします。
5. 認証後、Copilot Chat が表示されることを確認します。

   ![Copilot Chat の認証を求める画面の例](../../../_images/ex1-copilot-authentication.png)

## まとめ

ワークショップ用リポジトリのコピーを作成しました。また、コードを書く演習で使う codespace の作成を開始しました。

## 次のステップ

Copilot を使い始めましょう。プロジェクトの規約を伝える[演習 1: カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
