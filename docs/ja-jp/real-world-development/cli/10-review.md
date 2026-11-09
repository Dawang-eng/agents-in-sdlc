---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加し、ポート転送したブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 品質保証 (QA) のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルタリング機能全体を確認し、その PR で Agent Merge を使用しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認しました。

## 提出した内容

このワークショップには、それぞれ更新済みの `main` から作成した別々のブランチで進める、2 つの PR の節目があります。

1. **星評価**: ゲーム カードに既存の `starRating` と明示的な未評価状態を表示します。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連テストを追加します。

フィルタリングを計画してから PR を作成するまで、同じ会話とブランチを使いました。各ツールの使い方に集中できるように、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を使った Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果と、要件やテスト範囲のレビューをまとめました。

追加するテストは、実際に不足している範囲を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前に、コードと根拠を確認してください。変更を加えた場合は、影響を受ける検証を再実行してください。

## ベスト プラクティス

Copilot に提供するコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは、複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を検討する場合、**Interactive** は範囲を絞った変更を進めながら確認する場合、**Autopilot** は範囲が明確なタスクを任せる場合に使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは、インフラストラクチャと同じくらい重要です。構築する内容、その理由、実現方法を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI には、ほかにも試せる機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を整理し、別の視点から意見をもらう。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どのツールも、使い続けることで活用方法が上達します。実際のコード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで試してみてください。学んだことをチームで共有し、ほかのメンバーからも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、[Foundry Backer Concierge シリーズ][foundry]を任意で続けられます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
