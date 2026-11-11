---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。この環境で指示ファイルを編集し、cloud agent の作業結果をレビューします。

## ラボ用リポジトリを設定する

コードを作成するためのリポジトリのコピーとして、[テンプレート リポジトリ][template-repository]から新しいインスタンスを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![テンプレート リポジトリの設定](../../../_images/ex0-repository-settings.png)

4. 後で参照するため、作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。

## Codespace を作成する

次に、Codespace を使ってラボの演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を提供します。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタン](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して Codespace を作成します。

   ![Codespace の作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに短時間で済みます。作成を待つ間、次に紹介する GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 後の演習でこの Codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [開発コンテナー][dev-containers]で実行できます。どちらの環境にも、スムーズに進めるために必要なものがインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択します。Codespace と同じ開発コンテナーが VS Code で構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Codespace の作成を開始しました。この環境で Copilot cloud agent と一緒に作業します。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の開始方法][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
