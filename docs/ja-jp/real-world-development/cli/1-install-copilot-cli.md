---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンド ラインからタスクを任せたり、変更を依頼したりできるため、作業に集中できます。最初のステップは、Copilot CLI をインストールすることです。使い慣れたツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、Copilot との会話を試す。
- 組み込みの GitHub MCP サーバーを使ってフィルタリングの Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルからその機能を利用できます。このレッスンでは、残りのワークショップで使えるようにインストールと認証を行います。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI にアクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認して、**Yes, and remember this folder for future sessions** を選びます。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動した後は、自然な言葉で入力するとエージェントへの依頼になり、スラッシュ コマンドで会話を操作できます。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を 2 回押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が正しく動作していることを確認するため、次の簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーを利用できることを確認します。
6. 次のプロンプトで、フィルタリングの Issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の変更を行った後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree が作成されるわけではありません。変更を始める前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は既に許可された範囲の外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが選択するショートカットで、Codespace 内の承認プロンプトを省略し、演習に集中できます。

> [!CAUTION]
> `--yolo` はすべての操作に対する自動許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使い、日常的な開発の既定値として alias に設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を指定して、後のレッスンで Issue やプルリクエストを操作するための GitHub MCP の読み取り / 書き込みツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub リソースには実際にアクセスできます。変更を公開またはマージする前に確認してください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう 1 つ簡単な質問をし、会話が動作することを確認してから `/exit` を入力します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に `copilot --resume` を使い、同じ機能の会話とブランチを再開します。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次の操作を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルタリングの Issue を見つけました。

次は[最初の小さな変更を始め][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
