---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリング機能を確認しました。次は、これらの機能を明確な QA プロセスを備えたカスタム エージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認する。
- 再利用可能な品質保証プロファイルを作成して確認する。
- QA エージェントを選択し、フィルタリングの Issue と照らして結果を確認する。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使えば QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専用設定です。目的、指示、利用できるツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらの機能を要件の評価や結果の報告にどう使うかを Copilot に指示します。これらの機能を置き換えるものではなく、別の会話が必要になるものでもありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実施するチェックと、その作業範囲を明確に定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードになっていることを確認します。
2. 既定のエージェントに、次の新しいカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと制約を反映したか確認します。確認しておくと、検証だけを依頼したのに不完全または広範なエージェントが機能を変更する事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略できますが、設定するとエージェント名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告すること、実装コードを変更する前に確認を求めること、コミットやプルリクエストの作成を行わないことを確認します。
5. 必要な責任や制約が抜けていた場合は、既定のエージェントに修正を依頼してから次に進みます。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成したら同じフィルタリングの会話を再開し、QA を選びます。これにより、Issue と計画中に決めた内容を引き続き参照できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。その場合も、上記のコマンドで有効にできます。

2. 次のプロンプトで、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進めます。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その結果を確認しました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue と照らして結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらをまとめて機能の PR を[作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
