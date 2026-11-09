---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュ コマンドがあります。これらのコマンドから、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` を使って、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、クラウド エージェントへの委任に使うコマンドを確認する。

## シナリオ

CLI の主要なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッション共有などの機能を確認しましょう。[Copilot cloud agent][about-cloud-agent]への委任も任意で試せます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使ってコンテキストを確認したり、手動で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比べます。会話がまだ短い場合は、大きな変化がないこともあります。

> [!NOTE]
> コンテキスト ウィンドウの空きが少なくなると、Copilot CLI が自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関連のないタスクに切り替えて新しい会話を始めたい場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドでは、セッションを Markdown ファイルや HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで使える `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合う出力先を選びます。たとえば `/share file` を使うとローカルに Markdown としてエクスポートできます。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に動作してプルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の内容です。試すときは、新しい範囲の明確なタスクから始め、通常のワークフローで結果を確認してください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、CLI の内部状態を確認したりできます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使って、コンテキストの管理方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- cloud agent への任意の委任を含め、並行作業用のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュ コマンドがあります。次は、学んだ内容を[振り返り、今後のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
