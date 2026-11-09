---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで正しく動作するか確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストする。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にあるツールを利用できると、さらに便利になります。Copilot が外部のツールやサービスにアクセスするための標準的な方法が Model Context Protocol (MCP) です。

![エージェント モード、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信できるようにします。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムにやりとりできます。最新情報 (リソース) にアクセスしたり、ユーザーに代わって操作 (ツール) を実行したりできます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービスを仲介します。既存の API や npm パッケージなどのローカル ツールと通信します。それぞれの MCP サーバーから、異なるツールやリソースを利用できます。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue とプルリクエストの管理などを AI エージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright のブラウザー自動化機能を利用できます。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントに依頼できます。

ほかにも多くの MCP サーバーがあり、さまざまなツールやリソースを提供しています。GitHub は、エコシステム内の MCP サーバーを見つけたり、貢献したりできる [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの面では、MCP サーバーを他の依存関係と同様に扱ってください。使用前にソース コードを慎重に確認し、公開元を検証し、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot が Issue のバックログを読み取れました。この設定では[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はなく、VS Code が HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるようになります。機能をテストするために必要なツールです。

1. `.vscode/mcp.json` に `github` と並べて `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright はウィンドウを表示せずにブラウザーを実行します。デスクトップ環境がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys はエンドツーエンド テストに Playwright をすでに使っているため、通常は Playwright のブラウザーもインストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行してから再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` 設定の上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するか確認されたら、設定を確認し、サーバーが起動できるよう信頼することを選びます。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot に操作させる実際のブラウザーを提供します。利用者がアプリを操作して確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用し、結果を読み取って要約させることができます。会話を離れずに機能の動作を確かめる簡単な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を調べます。そのため、簡単な機能確認がアクセシビリティの基本チェックにもなります。

サーバーを接続し、アプリを実行した状態で、Copilot に作成したフィルタリング機能を試すよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して、結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。動作に問題があれば、プルリクエストを作成する前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> このテストには、`http://localhost:4321` で実行中のアプリが必要です。開発サーバーを停止した場合は、プロンプトを送信する前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が変更したファイルを確認します。
2. **+** アイコンを選んで変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更されたファイルが表示されている Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選び、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も任せましょう。Copilot は関連する Issue を見つけ、関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選んで新しいセッションを開始します。
2. エージェント選択メニューで **Agent** が選択されていることを確認します。GitHub ツールを使うために必要です。
3. 次のプロンプトで Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために必要な場合は **Continue** を選びます。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の指示に従ってください。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成します。
6. Copilot が生成したリンクを選び、プルリクエストを確認します。まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

機能を**テスト**するために MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
