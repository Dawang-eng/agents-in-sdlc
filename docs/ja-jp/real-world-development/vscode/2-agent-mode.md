---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は、変更が必要なファイルを探し、更新し、テストを実行し、デバッグし、更新が漏れたファイルを見つけ、さらに変更を加える、といった作業を行います。こうした手順は続いていきます。

このような場面で役立つのが Copilot Agent Mode です。

Copilot Agent Mode は、IDE 内でより自律的に作業するように設計されています。開発者と同じように、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチーム メンバーのように非同期で作業し、Issue とプルリクエストを通じてやりとりします。

この演習では、次の操作を行います。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定する。
- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を把握するため Tailspin Toys の Web サイトを確認します。

Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。サイトは、ビルド時にページを静的 HTML として生成する単一の Astro アプリです。ページでは、フロントマターから Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別個のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作には標準の DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の続きで、`main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選んで閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選ぶときは、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択します。

> [!NOTE]
> Codespace では、ターミナルに表示された localhost URL を選ぶと、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボード ゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選ぶと、そのタイトル、説明、パブリッシャー、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選んで新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

現在の Web サイトは動作しますが、さらに機能を追加したいと考えています。まず、バックログを確認しましょう。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント選択メニューで **Agent** を選びます。**Agent** はファイル全体の変更を計画して実装し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨する意図はありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくするためです。ただし、LLM は確率的に動作するため、受け取る提案が例と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を表示するよう依頼します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため、**Continue** を選びます。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって有用だと考える順に、Issue を優先順位付けしたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトに機能を追加する方法が記載された [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などの特定のファイル用の指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト ファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きなガイダンスを小さく管理しやすいファイルに分け、Copilot に参考資料やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも 3 つの別々の変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整する。
- フィルタリングの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースに変更をマージする前にテストを実行して合格する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選び、指示ファイルを指定します。

   ![UI 指示ファイルの選択例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のまま選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、関連ファイルを探す様子を確認します。データ レイヤーのヘルパー、UI、テストが見つかり、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は、プロジェクトの調査、変更、テスト実行などを行います。タスクやコードベースの複雑さによっては数分かかります。処理中に、エディターに **Keep** と **Undo** ボタンが表示される場合があります。Copilot が完了すると、すべての変更に **Keep** または **Undo** が表示されるため、作業中に選ぶ必要はありません。

7. Copilot から求められたら、**Continue** を選んでテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に何度か停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーが見つからなくなるまでタスクを進めます。

   ![Copilot Agent Mode との完了したチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選びます。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ レイヤーにまたがる機能を実装できる。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数のファイルを連携して変更できる。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲームの一覧が増えたら、ページネーションが必要です。この演習で学んだ方法を使い、Copilot にサイトのページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従う。
- ページ サイズを利用者が選べるようにするか、固定にするかを決める。
- 希望する実装に必要なガイダンスを Copilot に提供するプロンプトを作る。
- Copilot に変更を依頼し、コンテキストを提供しながら反復する。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
