---
title: "演習 1 - カスタム指示 (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行内容だけでなく方法も伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここでは変更前後を比較するプロンプトを実行しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムで並べて効果を示すのが難しいためです。この環境の後半で、cloud agent が作成したプルリクエストをレビューするときに指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc ドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot が理解するためのコンテキストと設定を提供します。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体、または特定のファイル タイプに指示を作成して、タスク単位のコンテキストを設定することもできます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の指示ファイルです。使用する技術スタック、開発内容の概要、ベスト プラクティスなど、ほとんどのチャットまたは CLI 要求に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル タイプ向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成や単体テストの追加といったタスクのガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
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

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対 glob を指定し、指示の適用先を決めます。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、コード生成時に従う例を Copilot に示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント化はまだ必須ではありません。次の手順で、TSDoc ドキュメント コメントとファイル ヘッダーのコメントに関する規則を追加します。

## リポジトリ全体に適用する標準を追加する

次に、この演習で行う唯一の編集として、TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーをコードに含めるプロジェクト全体の規則を追加します。cloud agent は、この環境の後半で割り当てる issue に取り組むときにこの指示を使います。

編集する前に作業ブランチを作成します。cloud agent は issue の対象ブランチから指示を読み込みますが、変更を個別にコミットします。

1. Codespace のターミナルで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関する規則はまだありません。
3. 既存の標準の直後に次の Markdown を追加し、ファイル ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - エクスポートするすべての関数に、目的、パラメーター、戻り値を説明する TSDoc コメントを記述します。
   - import 文やコードの前に、ファイルの目的を説明するコメント ブロックを追加します。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンスを記載するリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル タイプ、タスク向けの `*.instructions.md` としても作成できます。今回追加したドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は issue の対象ブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。そのため、cloud agent が指示を使えるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトでの作業方法に合わせてブランチを使います。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この環境の残りの演習で cloud agent が作成するプルリクエストに、その標準が反映されます。

次に、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot: 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
