---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、Copilot に特定の指示、ベスト プラクティス、コーディング パターン、専門知識を提供して、特定の作業をより効果的に行えるようにします。チームの知識を再利用可能なエージェントにできます。たとえば、Web Content Accessibility Guidelines (WCAG) 準拠を確認するアクセシビリティ エージェント、安全なコーディングを実践するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` で定義します。各ファイルには、少なくとも `name` と `description` を含む YAML (YAML Ain't Markup Language) フロントマターがあり、その後に動作、専門知識、指示を定義する Markdown プロンプトが続きます。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に操作の実行方法を伝えます。最も分かりやすく言えば、**カスタム エージェント**は作業者で、**スキル**はそのツールです。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整するように設計されています。この演習では、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルやテストを実行、管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI のほかの機能と同様、環境やシナリオに合う方法が見つかるまで試して調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントでは、次の内容を確認します。

- カスタム エージェントの定義方法
- カスタム エージェントにタスクを割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを使えるように取り組んでいます。最近のフィードバックによると、テキストと背景のコントラストが不十分なため、現在のダーク テーマが読みにくいと感じるユーザーがいます。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装したいと考えています。機能の生成にはカスタム エージェントを使います。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認し、Copilot への指示を把握しましょう。

Codespace に戻り、アクセシビリティのカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシブルな Web サイトのコード生成に関する主な責任
   - アクセシビリティのベスト プラクティス
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

エージェントのミッション コントロールは、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当てて進捗を監視したり、セッションの進行中に方向を変えたり、追加の指示を出したりできます。まず、Copilot に高コントラスト モードを作成するタスクを割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   サイトに高コントラスト モードが必要です。ユーザーが高コントラストを切り替えられるトグルを用意してください。設定はブラウザーの local storage に保存してください。
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当て用のダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

この演習では、特定のタスクや分野に合わせた AI アシスタントである GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの知識や標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるようにできます。

次の内容を確認しました。

- カスタム エージェントの定義方法
- カスタム エージェントにタスクを割り当てる方法

Copilot が高コントラスト モードを実装している間に、[ミッション コントロールからエージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
