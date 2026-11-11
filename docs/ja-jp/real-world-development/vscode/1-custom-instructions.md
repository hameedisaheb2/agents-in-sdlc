---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。コンテキストを共有する方法はいくつかあります。その 1 つが[指示ファイル][instruction-files]です。指示ファイルでコード生成の指針を Copilot に伝えます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- *現在の*指示が設定された状態で、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の更新とフィルター機能のデータ部分を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot の出力は確定的ではありません。この演習では、指示の更新後に動作がどのように*変化する傾向にあるか*を確認します。出力を一字一句同じにすることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ず単体テストを用意します。
- ユーザー インターフェイス (UI) はダーク モードにし、現代的な見た目にします。
- コードには TypeScript Documentation (TSDoc) コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot が理解するためのコンテキストと設定を提供します。Copilot に適切な提案やコード スニペットを生成させるための強力な機能です。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のファイル タイプに指示を作成して、タスク単位のコンテキストを設定することもできます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の指示ファイルです。使用する技術スタック、開発内容の概要、ベスト プラクティスなど、ほとんどのチャットやコマンドライン インターフェイス (CLI) の要求に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル タイプ向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成や単体テストの追加といったタスクのガイドラインを指定できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく指示ファイルもサポートします][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法を示す例が含まれています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発内容、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル タイプや、特定のタスクに関する指示を記載します。
- 自然な言葉で明確に記述します。コードの良い例と避けるべき例を示します。

指示ファイルの作成方法にも AI の使い方にも、唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットが必要です。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを作成させることもできます。各環境で方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使用します。利用中の環境に関係するレッスンで、必要に応じて説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] リポジトリを確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対 glob を指定し、指示の適用先を決めます。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、コード生成時に従う例を Copilot に示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc ドキュメント コメントとファイル ヘッダーのコメントに関する規則を追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新して同じプロンプトを再送し、結果の違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、すべての出版社の一覧を返すデータ アクセス ヘルパーを作成または更新してください。各出版社について名前と id を返してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの出版社配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
7. 提案されたコードには TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確定的ではないため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の*一貫性*の改善が、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関する規則はまだありません。そのため、生成されたヘルパーにはドキュメント コメントがありませんでした。
3. 既存の標準の直後に次の Markdown を追加し、ファイル ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの目的を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル タイプ、タスク向けの `*.instructions.md` としても作成できます。今回追加したドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 同じプロンプトを再送して変化を確認する

リポジトリ全体に適用する標準を追加したので、同じプロンプトをもう一度送信して結果を比較します。

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、すべての出版社の一覧を返すデータ アクセス ヘルパーを作成または更新してください。各出版社について名前と id を返してください。ファイルに変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys Crowd Funding プラットフォーム用の出版社データ アクセス ヘルパー。
    * データベースから出版社情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * すべての出版社について id と名前を含む一覧を返します。
    *
    * @param db - Drizzle データベース クライアント。
    * @returns 出版社オブジェクトの配列に解決される Promise。
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で基盤として使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (たとえばテスト) が表示される場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、ドキュメント標準の issue に関連付けたプルリクエスト (PR) を作成します。GitHub.com から手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Copilot に issue を検索させ、issue をクローズする PR を作成してからマージします。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を実行します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。設定は不要ですが、Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。この接続の仕組みである Model Context Protocol (MCP) は、後の演習で確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を検索し、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と出版社ヘルパーの基盤の両方を記載します。

   ```
   指示ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その PR が issue をクローズすることを明記してください。また、次のフィルター作業に向けて出版社ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が issue の検索と PR の作成を開始します。
5. Copilot が GitHub 操作を代わりに実行する許可を求めたら、コマンドを確認し、適切であれば **Allow** を選択します。
6. PR が作成されたら、PR を `main` にマージし、ローカル ブランチを `main` に戻して最新コードを取得するよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新コードを pull して更新してください。
   ```

7. Copilot が GitHub 操作やシェル コマンドを代わりに実行する許可を求めたら、コマンドを確認し、適切であれば **Allow** を選択します。

GitHub Copilot を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送し、提案されたコードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で作業を続けられるようにしました。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体の新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot: 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
