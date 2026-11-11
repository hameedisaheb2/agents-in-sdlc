---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルターを構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示やパス指定の指示ファイルがエージェントに届く仕組みを確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントではコードを言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。`db` 引数を注入できる場合は、そのことも記載します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載します。関連コードが変わったらコメントも最新の状態にします。
- 現在のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発手法に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディング スタイルや要件を Copilot が理解するためのコンテキストと設定を提供します。コーディング規約、ライブラリ、コードに含めるコメントの種類を指定できます。リポジトリ全体または特定のファイル タイプ向けの指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**要求で Copilot に送信される単一の指示ファイルです。ほとんどの要求に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル タイプ、タスクに関するガイドラインを提供します。

> [!NOTE]
> 指示ファイルの形式とサポート範囲は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、作業を始めやすいよう指示ファイルが含まれています。変更前に既存のファイルを確認し、変更の効果を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにあるファイルは良い出発点ですが、まだ不足があります。新しく生成する TypeScript に TSDoc コメントを含めるため、中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして次を追加します。

   ```markdown
   - 新しい TypeScript コードには、ドキュメント用の TSDoc コメントを含めます。
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルターの会話を終了して再開します。機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルターの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチにあるリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   指示とコード ガイダンスを更新しました。生成したコードをそのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示やパス指定の指示ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準が守られるように指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
