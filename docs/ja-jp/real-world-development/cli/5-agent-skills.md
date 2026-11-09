---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズしてフィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードが動作することを確認し、指示ファイルで標準に従うようにしました。テスト、lint、その他の継続的インテグレーション (CI) はどうすればよいでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、こうした操作の正しい実行方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認する。
- 結果の形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースが含まれます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキル名と説明を定義するフロントマター、スキルの概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、フォルダーにスクリプトやその他のリソースを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにあるコマンドを使うため、`SKILL.md` だけで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、リポジトリのアセットとしてチームで共有し、再利用できます。ユーザー スキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明を基に、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、Copilot がテストと lint を実行する手順を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## スキルを変更する前に実行する

スキルは Copilot CLI から直接呼び出すことも、自然な言葉で依頼することもできます。3 つのチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードのフィルタリング会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際にどのような結果を報告したかが分かる、より良いレポートを作成します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを試します。Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリング機能の PR に含めます。次は、[Playwright MCP サーバー][next-lesson]を通じて Copilot がサイトを直接操作できるようにします。

## その他のスキル例

コミュニティの次の例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
