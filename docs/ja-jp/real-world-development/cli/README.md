---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、GitHub Copilot をターミナルで使えるエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから操作できるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローを進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトの概要を把握します。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリング Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリング機能を確認します。
7. 品質保証 (QA) のカスタム エージェントを作成して使い、要件、テスト範囲、検証結果をレビューします。
8. フィルタリング機能全体を確認し、Agent Merge を使います。
9. コンテキスト、モデル、共有、任意のクラウドへの委任に使う便利なスラッシュ コマンドを確認します。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。各ツールを学ぶ間、フィルタリングと品質のワークフローを 1 つの会話とブランチで進められるようにしています。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い始める |
| [2. 星評価を追加する: 最初の小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive で確認する |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルタリング機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルとレポート形式を確認、変更し、実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI の MCP を構成し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選び、最終的な検証結果を集める |
| [8. 機能のプルリクエストを作成してマージする][ex8] | レビューとマージ | 変更全体を確認し、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、クラウドへの任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考資料を確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントを構築してデプロイし、サイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
