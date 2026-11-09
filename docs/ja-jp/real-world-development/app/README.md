---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は、Copilot CLI を基盤としたデスクトップ アプリケーションです。エージェント主導の開発を、1 つの集中できるワークスペースで進められます。複数のエージェント セッション、切り替え可能なセッション モード、共有キャンバス、GitHub Issue とプルリクエストのネイティブな管理機能を備えています。**Agent Merge** を使うと、プルリクエストのリベース、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを自動化できます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールして、リポジトリを接続し、ワークスペースと登録済みのバックログを確認する。
2. 星評価の小さな変更を行い、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージする。
3. フィルタリング Issue から始め、**Plan** モードで方針を決め、**Autopilot** モードで実装し、**Interactive** モードでレビューする。
4. リポジトリの指示を更新し、フィルタリングの作業に適用する。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行する。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリング機能を確認する。
7. 品質保証 (QA) のカスタム エージェントを作成して使い、要件、テスト範囲、検証結果をレビューする。
8. フィルタリング機能全体を確認し、2 つ目の PR で Agent Merge を使う。
9. キャンバスが利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージ キャンバスを作成してテストする。必要な機能を利用できない場合は、その制約を記録し、読み取り専用の代替方法を使うか、まとめに進む。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。どちらも更新済みの `main` から始めます。フィルタリングと品質のワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学ぶ間に作業を活用できます。最後のキャンバス演習は同じセッションで進め、PR ワークフローを繰り返さずに、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価を追加する: 最初の小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive で確認する |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルタリング機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を構成し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選び、最終的な検証結果を集める |
| [8. 機能のプルリクエストを作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストを確認し、2 つ目の PR で Agent Merge を使う |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ キャンバスを作成してテストする |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、参考資料を確認する |

## 前提条件

このワークショップに参加する前に、次のものを準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。Copilot CLI のポリシーとは別の設定です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
