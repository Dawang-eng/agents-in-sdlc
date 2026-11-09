---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せ、開発を加速し、より重要な作業に集中できます。

Copilot cloud agent を使って、次の操作を行います。

- コード生成の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- Issue の範囲を明確に定めることの重要性を確認する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを構築した請負業者が、ドキュメントを十分に作成していません。まず、アプリケーションのエクスポート関数すべてに TSDoc の doc コメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示し、利用者が次のゲームを探し続けられるようにします。現時点では完成された実装ではなく、ユーザー受け入れテストに使えるものを求めています。この作業はブロッカーですが、優先度の高い他の Issue もあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、開発者が別の作業に取り組んでいる間に非同期で処理できます。後で Copilot の作業をレビューし、期待どおりの結果になっているか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当て後、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。処理中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決方法のレビュー準備ができると、Copilot がプルリクエストで通知します。

## 適切な範囲を持つ指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく見ると複雑な部分があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示とその役割を確認しました。カスタム指示の演習をすでに完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読む良い機会です。後で、指示がどのように影響したかを確認できます。

GitHub の Web UI でリポジトリを開くか、Codespace を使用して、次のファイルを読み取り専用で確認します。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc の doc コメントに関する指針を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てると、これらの指示ファイルから TypeScript の規約、TSDoc の doc コメント、コメント ヘッダーが反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent 用の開発環境を設定する

コードを作成する際は、通常、適切な環境とセットアップが必要です。タスクを Copilot に割り当てる場合も同じです。Copilot はソフトウェア エンジニアのように作業を実行します。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に [特別なセットアップ ワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係をインストールできます。このワークショップでは、学習をスムーズに進められるよう、すでに設定されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # このステップで必要となる最低限のアクセス許可を設定します。Copilot の操作には専用トークンが使われます。
    permissions:
      # セットアップ手順内でリポジトリをクローンする場合 (依存関係のインストールなど) は、`contents: read` が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
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

      # ビルドとテスト用にローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これにより、Copilot の実行を待たずに、リポジトリの **Actions** タブから手動でワークフローを実行できます。セットアップが正しく動作するか試すときに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、ほとんどのプロジェクトでは情報が古かったり、十分に記載されていなかったりします。こうした技術的負債は放置されることが多く、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成に役立つため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成します。後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選びます。
3. **New issue** を選び、新しい Issue のダイアログを開きます。
4. **Blank issue** を選び、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選び、Issue を作成します。
8. 右側にある **Assign to Copilot** を選び、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選びます。
10. **Pull Requests** タブを選びます。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。新しい PR が表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかる場合があります。Copilot は、事前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に対する最適な対応方法を判断しています。

13. TODO リストと、Copilot が完了しようとしているタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選びます。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザー ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot の作業を確認します。コードを調査して状態を把握し、計画を考え、コードを作成する様子を確認できます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを実行し、開発者が別の作業に取り組めるようにすることです。この機能を使い、もう 1 つのタスクも cloud agent に割り当ててから、アプリに機能を追加するコードを書きます。

## ゲームの詳細ページに関連ゲーム セクションを追加する

先ほど説明したとおり、Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チームが求める関連ゲーム セクションは、長時間かからないかもしれません。それでも、その時間をほかの作業に使えます。Copilot cloud agent にタスクを割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選びます。
3. **New issue** を選び、新しい Issue のダイアログを開きます。
4. **Blank issue** を選び、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選び、Issue を作成します。
8. 右側にある **Assign to Copilot** を選び、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選びます。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Issue に Copilot が作業中であることを示す目の絵文字が表示されている](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、完了するまで常に監視する必要はありません。完了後にレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業させることができます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに活用できます。

この演習では、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作が安全に行われるようにする方法を確認しました。
- Issue の範囲を明確に定めることの重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。カスタム指示については前のモジュールで確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

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
