---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにある特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業に必要な指示、ベスト プラクティス、コーディング パターン、専門知識を提供できます。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を説明する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。違いを簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー スキルやテスト実行・管理スキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の他の機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、文字と背景のコントラストが十分でないため、現在のダーク テーマを読みにくいと感じる利用者がいます。このアクセシビリティの課題に対応するため、デザイン チームは利用者が切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタム エージェントに機能を生成させましょう。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装する。

## アクセシビリティ用のカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認して、Copilot への指示を把握しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します。演習 3 のフィルタリング PR とは分けて進めます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択メニューには、`.github/agents` で定義されたカスタム エージェントが表示されます。エージェントを選ぶと、会話でそのエージェントの指示とツールを使えます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選んで新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント選択メニューを選びます。アクティブなエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選びます。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクト ファイルを編集します。編集ごとに、ファイル パスと差分がチャットに表示されます。
6. エージェントが変更したファイルに合わせて **Files changed** の表示が更新されることを確認します。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は実際のリポジトリを変更し、Astro コンポーネント、CSS、関連テストなどを更新します。

次の演習では、作業中の変更を確認し、エージェントに追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専用の AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業をより適切に進められるようにします。

この演習では、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat のエージェント モードでカスタム エージェントを使いました。

次は、エージェントの作業を[監視して調整します][next-lesson]。作業中に変更をレビューし、同じセッションにライト モードの切り替えを追加するよう依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
