---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな形があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に行います。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent では、次の内容を確認します。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- issue の範囲を明確にする重要性
- Copilot に issue を割り当てる方法

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作成した請負業者は、ドキュメントを適切に整備しませんでした。実際、ドキュメントがまったくないことに気付くでしょう。まず、アプリケーションのエクスポートされたすべての関数に TypeScript Documentation (TSDoc) コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。ユーザー エクスペリエンス (UX) の受け入れテストに使えるものを求めています。この作業は現在ブロックされていますが、ほかに優先度の高い issue があります。

どちらも後回しになりやすいタスクであり、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期に作業している間、開発者はほかのタスクに集中できます。完了後に戻って作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者と同じように、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト PR を作成し、環境を準備して、タスクに取りかかります。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、PR であなたに通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot は魔法ではありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進めながら、学習、実験、調整を続けます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作を導く方法を確認しました。カスタム指示を使った演習をすでに行った場合は、`.github/copilot-instructions.md` の効果を確認したはずです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後でその影響を見分けやすくなります。

リポジトリの GitHub Web UI、または起動中の Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc コメントに関する内容に注目してください。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるとき、生成される PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。これらの指示は指示ファイルに由来します。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、通常、適切な環境を用意し、すべてが正常な状態になるようセットアップ スクリプトを実行します。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。作業開始前に実行する[専用のセットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。これにより、必要な開発ツールと依存関係を利用できるようになります。このワークショップでは、学習の流れを支えるために事前設定されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、単一の Astro アプリ用にマイグレーションと初期データ投入を済ませた SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンドとアプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるよう、ローカル SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。Copilot が PR の作業を始める前に GitHub Actions が実行します。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントがなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、割り当てるのに最適な issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   関数やメソッドには、役立つ場合に TSDoc ドキュメント コメントを付けるという要件があります。しかし、最近の更新ではこの標準が守られていません。動作を明確にする箇所にドキュメント コメントを含めるよう、既存コードを更新する必要があります。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用のダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後には Copilot が TODO リストを作成しているはずです。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は、前に説明したワークフローを実行して環境を準備し、プロジェクトを分析して、問題への最適な対処方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. PR のタイムラインを下にスクロールします。Copilot が issue に取り組み始めたことを示す更新が表示されます。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot の作業を確認します。コードを調べて状態を理解し、適切な計画を考えて決定したうえで、コードを作成します。

完了まで数分かかることがあります。Copilot cloud agent の大きな利点は、作業を非同期に任せてほかのタスクに集中できることです。この機能を活用し、もう 1 つのタスクを cloud agent に割り当てている間に、アプリケーションへの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームのセクションを追加する

これまでに説明したように、GitHub Copilot cloud agent の大きな利点は作業を分けられることです。デザイン チーム向けの関連ゲームのセクションは、時間がかからないとしても、その間にほかのタスクを進められます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   ゲーム詳細ページに関連ゲームを表示して、ほかのゲームを見つけやすくしたいと考えています。デザイン チームはユーザー エクスペリエンス (UX) を検討し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータ アクセス ヘルパーを追加する。このヘルパーは指定されたゲームと同じカテゴリのほかのゲームを返し、指定されたゲーム自体は除外する
   - ヘルパーを使って、ゲーム詳細ページに「Related games」セクションを表示する
   - 関連ゲームがない場合に対応する
   - 新しいヘルパーの単体テストを作成する
   - PR を作成する前に、すべてのテストが成功することを確認する
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用のダイアログを開きます。

   ![Copilot を issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

ほどなくして、issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めたことを示しています。

![Copilot が issue に取り組んでいることを目の絵文字で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューすればよいのです。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

この演習では、[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当て、非同期に処理させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに Copilot を活用できます。

次の内容を確認しました。

- コード生成環境をカスタマイズする方法
- 安全に操作を実行する方法
- issue の範囲を明確にする重要性
- Copilot に issue を割り当てる方法

Copilot cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使用しましょう。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも使用でき][cloud-agent-mcp]、カスタム指示も利用できます。これらは前のモジュールで確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
