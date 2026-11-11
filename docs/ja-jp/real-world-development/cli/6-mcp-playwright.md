---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くだけでは十分ではありません。データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにする必要もあります。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot が利用できるツールやサービスを追加できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使用するかを理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を調べます。

## シナリオ

単体テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにし、変更が期待どおりに動作するかを自動的に確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリアルタイムで外部ツールやサービスと通信し、最新情報にアクセスしたり、ユーザーに代わって操作したりできます。

ツールやリソースには MCP サーバー経由でアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの橋渡しをします。各サーバーは、エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけて貢献できるよう [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が構成されている場合は、設定手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバー タイプが **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. 次の文字列を **Command** ダイアログに貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を調べさせる

これまでは機能が期待どおりに動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   アプリを起動し、Playwright MCP を使って issue と計画に照らしてフィルターを確認してください。変更は加えず、動作する点としない点を教えてください。完了したら、起動したサーバーを停止してください。
   ```

> [!NOTE]
> 通常、Copilot は自分で MCP サーバーの使用方法を判断するため、明示的に指定する必要はありません。ただし、使用するサーバーが分かっている場合は伝えると、結果の一貫性が高まり、トークンを少し節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を見ます。
3. レポートを読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を調べました。このレッスンでは、次の操作を行いました。

- MCP とは何か、Copilot CLI がどのように使うかを学びました。
- Playwright MCP サーバーを利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を調べました。

次は、スキルとブラウザー ツールを専門的な役割にまとめる[QA カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
