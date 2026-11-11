---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、フィルターと品質に関する作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge とは何か、マージ ライフサイクルをどのように自動化するかを学びます。
- 機能の変更全体と検証の証拠を確認します。
- フィルターの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルターのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認を開発者が管理しながら、PR の残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR のブロック要因に対応します。失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースを行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。Agent Merge にこの処理を任せることもできます。有効化する前に、作業をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功するとマージを実行します。
7. Copilot が `"PR #14 was squash-merged successfully."` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可される場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発作業の一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge とマージ ライフサイクルの自動化について学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI スラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
