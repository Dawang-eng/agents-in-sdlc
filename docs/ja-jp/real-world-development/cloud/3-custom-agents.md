---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローにある特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業に必要な指示、ベスト プラクティス、コーディング パターン、専門知識を提供できます。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントなどです。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を説明する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。違いを簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフロー スキルやテスト実行・管理スキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の他の機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の操作を行います。

- カスタム エージェントの定義方法を確認する。
- カスタム エージェントにタスクを割り当てる。

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべての利用者がクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックによると、文字と背景のコントラストが十分でないため、現在のダーク テーマを読みにくいと感じる利用者がいます。このアクセシビリティの課題に対応するため、デザイン チームは利用者が切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタム エージェントに機能を生成させましょう。

## アクセシビリティ用のカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに用意されています。内容を確認して、Copilot への指示を把握しましょう。

Codespace に戻り、アクセシビリティ用カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主要な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加の指示を送れます。まず、ハイコントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選びます。
3. **New issue** を選び、新しい Issue のダイアログを開きます。
4. **Blank issue** を選び、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選び、Issue を作成します。
8. 右側にある **Assign to Copilot** を選び、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選びます。

    ![カスタム エージェントとアクセシビリティ エージェントが選択されている cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選びます。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専用の AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が作業をより適切に進められるようにします。

この演習では、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- カスタム エージェントにタスクを割り当てました。

Copilot がハイコントラスト モードを実装している間に、次は mission control から[エージェント セッションを監視して調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
