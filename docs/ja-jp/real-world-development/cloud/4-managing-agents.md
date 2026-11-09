---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでのいくつかの演習では、ユーザー エクスペリエンスの改善や機能の追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに指示を送り、機能を追加する。

## シナリオ

ハイコントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、自然な追加機能です。ハイコントラストに加えてライト モードも作成するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインに表示されたタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲーム変更用 API の生成。
   - Web サイトへのハイコントラスト モードの追加。
3. 実行中のタスクを 1 つ選び、Copilot が実行した操作を確認します。たとえば、次のような操作があります。
   - リポジトリをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するための各種操作を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、追加で作業を監視できます。

## cloud agent の作業を調整する

現在実行中のタスクを確認できました。次は、エージェントがハイコントラスト モードを作成している間に、ライト モードの切り替え機能も追加するよう依頼します。

1. ハイコントラスト モードを追加するセッションを選びます。タイトルは、Copilot が付けた名前や現在の作業状態によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションを調整するダイアログが表示された agents ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に含めたことを確認します。

## Copilot に作業を任せる

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順が終わると、新しい依頼をワークフローに追加します。

今回も数分かかります。作業が終わるまで、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページについて学びました。この mission control では、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクに指示を送って範囲を調整できます。

このレッスンでは、次の操作を行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
