---
title: "レッスン 8 - 機能のプルリクエストを作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリング機能、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージ作業を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質に関する一連の作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学ぶ。
- 機能の変更全体と検証結果を確認する。
- フィルタリングの PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、開発者がマージを許可する権限を保ったまま、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題を解決します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な場合のリベースなどを行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられますが、有効にする前に内容をレビューして、マージ可能であることを確認してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. オプションの一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリの設定、権限不足を回避しません。ブロックされた場合は、報告された理由を確認してください。リポジトリで許可されている場合は、レビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスのいくつかの部分を自動化しました。次の操作を行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
