---
title: "レッスン 2 - 星評価を追加する: 最初の小さな改善"
description: "Copilot CLI でゲーム カードを変更し、ポート転送したブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに初めて変更を加えます。小さな変更から始めましょう。各ゲームのデータには星評価が含まれていますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更を確認し、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで Copilot の会話を始める。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- ポート転送したブラウザーでアプリを実行し、変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードには、タイトル、カテゴリ、パブリッシャー、説明しか表示されていません。最初のセッションにぴったりな小さく独立した変更として、既存の評価を各カードに表示するようエージェントに依頼します。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI の会話では、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使用し、専用の worktree は作成しません。保存した会話は後で再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

新しい会話を始めて、機能の実装に取りかかります。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ Copilot が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。これで Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態がカバーされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは、決定論的ではなく確率的に動作するため、コードは例と異なる場合があります。完全に同じ実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読んだだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces でポート転送したブラウザーから更新されたユーザー インターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選びます。
3. 各ゲーム カードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、コードをプロジェクトにマージするためのプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI が表示した URL を選択して PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 準備ができたら **Merge pull request** を選び、マージを確定します。
6. Codespace に戻り、`/exit` を入力して Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースできました。次の操作を行いました。

- 機能ブランチで Copilot の会話を始めました。
- ゲーム カードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、ポート転送したブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルタリング Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
