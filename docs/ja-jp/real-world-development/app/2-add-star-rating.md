---
title: "レッスン 2 - 星評価を追加する: 最初の小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを試したので、次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータには星評価がすでに含まれていますが、ホームページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェント セッションを開始し、その構成を理解する。
- エージェントにプロジェクトの小さく対象を絞った変更を依頼する。
- ワークスペースの差分ビューで変更をレビューする。
- アプリをローカルで実行し、ブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲーム カードに表示されるのは、タイトル、カテゴリー、パブリッシャー、説明だけです。最初のセッションの準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した変更なので、最初のセッションにぴったりです。

## セッションの構造

**セッション**とは、分離された独自のワークスペースでエージェントと会話する場です。各セッションには専用の **Git worktree** とブランチが割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数の作業を同時に進められます。セッションはサイドバーでリポジトリごとにグループ化され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、エージェントがファイルを調査および編集する際の**ツール アクティビティ**、差分を確認できる**変更済みファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを調べて機能を実装します。[アプリのセットアップ][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼しましょう。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選んでセッションを開始します。
3. リポジトリとして `tailspin-toys` を選びます。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選びます。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、新しい worktree (プロジェクトの分離されたコピー) を作成して作業を始めます。次にプロジェクトを調べ、新機能を追加するために更新が必要なファイルを特定し、必要なコードを作成します。Copilot app で新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot app で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選びます。開いたパネルで **Changes** を選び、変更されたファイルとその差分を確認します。

    ![「Create PR」の右側にある「Toggle review panel」ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら "No rating yet" と表示する、次のような小さなブロックが追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、おおむね同じ内容になっているはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果を確認します。数値の `starRating` と、`null` の場合の代替表示をテストで確認していることを確かめてください。前提条件が不足していたり、チェックがスキップされていたりする場合は、成功とはみなせません。インストールを求められた場合は、承認前に内容を確認してください。

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新された UI を確認しましょう。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを調べ、自分でも作成します。ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトを使って、アプリを起動し、ブラウザー キャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価のあるゲーム カードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使って、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選びます。
2. 求められた場合は **Sign in with your browser** を選び、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR の作成前に関連する Issue が必要だと Copilot が伝えた場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選び、レビューパネルで PR を開いて内容を確認します。必要に応じて、ここで PR をレビューできます。
6. 準備ができたら **Ready to merge** を選びます。
7. 表示されたダイアログで **Merge pull request** を選び、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースできました。具体的には、次の操作を行いました。

- エージェント セッションを開始し、その構成を理解しました。
- エージェントにゲーム カードの小さく対象を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、自分でマージしました。

次は、[フィルタリング Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
