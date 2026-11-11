---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。具体的には、次の操作を行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、用意されていたフィルターの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの issue から作業を始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証証拠を評価しました。
- フィルターの変更全体をレビューし、フィルターの PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## 提供した内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を完了します。

1. **星評価:** 既存の `starRating` と未評価の状態をゲーム カードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、要件とテスト カバレッジのレビューにその結果をまとめました。

追加するテストは、実際に不足している部分を補う必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前にコードと証拠をレビューし、変更後は影響する証拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業の内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを複数の会話で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を検討するときは **Plan**、範囲の限られた変更を一緒に進めるときは **Interactive**、範囲が明確なタスクを任せるときは **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選択します。

コンテキストは、インフラストラクチャと同じくらい重要です。何を、なぜ、どのように作りたいかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主要なワークフローを終えました。Copilot CLI の次の機能も確認してみましょう。

- `/review` でコード レビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点からの提案を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることで活用方法が上達します。実際のプロダクション コードや個人プロジェクト、以前から作りたかった小さなアプリで使ってみてください。学んだことをチームと共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent のワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
