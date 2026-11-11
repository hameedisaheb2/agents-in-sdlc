---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このワークショップでは、ユーザー エクスペリエンスの改善や機能追加に関するいくつかのタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チームが試せる関連ゲーム機能の作成、高コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

これまでに何度も説明したように、生成型の人工知能 (AI) を導入してもソフトウェア設計や開発と運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスと、コードを作成するブランチへの**書き込みアクセスのみ**を持ちます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントを確認する

最初に、GitHub Copilot cloud agent が作成した 1 つ目の PR、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は外部 Model Context Protocol (MCP) サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TypeScript Documentation (TSDoc) コメントなどが含まれています。変更内容は状況によって異なります。

   TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらは演習 2 の冒頭で確認したカスタム指示ファイルによるものです。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot と作業するのは一方通行ではありません。チーム メンバーに依頼するのと同じように、PR やコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを新たに開始します。結果は確定的ではないため、必ず依頼すべき文面を指定することはできません。変更を依頼する例をいくつか紹介します。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する
- TypeScript と Astro のファイルに TSDoc コメントを追加する
- Astro アプリの構造を説明する README を作成する

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。上記の例を使うか、コードベースに追加したいドキュメントについて別の変更を依頼します。
2. **View Session** を選択して、Copilot が作業する様子を確認します。変更を行うため、新しいセッションが開始されます。
3. **Back to pull request** を選択して PR に戻ります。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更を確認します。

納得できるまで反復して改善してください。問題がなければ、ドラフト PR をレビュー可能な状態にして、main ブランチにマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能を確認する

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターが表示されます。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc ドキュメント コメントを追加したりできます（カスタム指示を更新する**前**に issue を割り当てたことを思い出してください）。前と同様に、**Conversation** タブにコメントを追加すると、Copilot がそれを確認して新しいセッションを開始します。

## アクセシビリティ機能を確認する

最後に、アクセシビリティのカスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 でミッション コントロールから依頼したライト モードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、次の点に注意します。
   - モードを切り替えるユーザー インターフェイス (UI) コンポーネント
   - ユーザー設定を保存する local storage の使用
   - 高コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード ナビゲーションなど)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターが表示されます。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティのカスタム エージェントは、アクセシビリティのベスト プラクティスに沿って Copilot が機能を実装するよう導きました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 作業を引き続き委任する

cloud agent に実際のバックログ項目を割り当て、ほかの作業に集中できると便利です。この方法に慣れるために、リポジトリにいくつか issue を作成して Copilot に割り当ててみましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する
- ゲーム一覧ページにページネーションを実装する
- データ アクセス ヘルパーに入力検証とエラー処理を追加する

## まとめ

Cloud agent 環境を完了しました。これらのレッスンでは、次の操作を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力にどのように反映されるかを確認しました。
- **Copilot cloud agent に issue を割り当て**、環境の準備、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタム エージェントを作成して使用し**、高コントラスト モードとライト モードを追加しました。
- **Copilot エージェント ページをミッション コントロールとして使用し**、アクセシビリティ セッションを監視して方向付けました。
- **cloud agent の PR をレビューして改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent 環境を完了しました。引き続き学習する場合は、ほかの環境で今回の内容をさらに実践できます。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE で Copilot Chat のエージェント モードと Model Context Protocol (MCP) 統合を直接確認します。
- 💻 **[コマンドライン インターフェイス (CLI) 環境](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、cloud agent へ戻る省略可能な `/delegate` ブリッジを使い、レビュー済みの変更を 2 つターミナルから作成します。

自分のリポジトリでは、次の作業を試してください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる
- 別の分野向けのカスタム エージェントを作成する
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
