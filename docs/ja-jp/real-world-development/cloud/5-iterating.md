---
title: "演習 5 - GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能の追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を実装し、ハイコントラストとライト モードの切り替えを含むアクセシビリティ機能を作成するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ人間の監視なしにタスクを実行するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのはコード用ブランチだけです。
- cloud agent は GitHub Actions 内で、作業用の一時的な環境を別に作成して実行します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選び、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選び、変更を確認します。

    ![「Files changed」タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc コメントなど、更新されたコードとドキュメントを確認します。実際の変更は環境によって異なります。

   変更を確認するときは、TSDoc の doc コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルで指定した内容です。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選びます。

   ![「Approve and run workflows」ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションに、ワークフローがキューに追加されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストで協力する方法は、変更を一方的に受け取るだけではありません。チーム メンバーに依頼するのと同じように、プルリクエストのコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、追加のセッションを開始して対応します。結果は確率的に変わるため、指定の文面はありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc の doc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションし、生成されたドキュメントの変更をコメントで依頼します。上記の例を使うか、コードベースに追加したいドキュメントについて別の変更を提案します。
2. **View Session** を選び、Copilot の作業を確認します。変更を適用するために Copilot が新しいセッションを開始します。
3. **Back to pull request** を選ぶと、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選び、変更を確認します。

満足するまで、必要に応じて繰り返します。準備ができたら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージできます。

![レビュー準備ができた状態に PR を変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選びます。
3. **Show related games on the game details page** などのタイトルの PR を選びます。
4. **Files changed** タブを選び、生成されたコードを確認します。
5. 更新内容に問題がないことを確認したら、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選びます。

   ![「Approve and run workflows」ボタン](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションに、ワークフローがキューに追加されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。例: **copilot/fix-8**

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent に新しいコメントを追加して、変更を繰り返し依頼できます。たとえば、表示する関連ゲームの数を変更したり、コメント ヘッダーと TSDoc の doc コメントを追加したりできます (この作業は、カスタム指示を更新する前に依頼したことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選びます。
3. **Add high contrast mode to website** などのタイトルの PR を選びます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選び、生成されたコードを確認します。
5. 実装をレビューします。特に、次の内容を確認してください。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存する local storage の使用
   - ハイコントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを処理する JavaScript / TypeScript のコード

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選びます。

   ![「Approve and run workflows」ボタン](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションに、ワークフローがキューに追加されたことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でブランチに切り替え、アクセシビリティ機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラスト モードとライト モードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティ用のカスタム エージェントが、Copilot の機能実装をアクセシビリティのベスト プラクティスに沿ってガイドしたことを確認します。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent は、実際のバックログのタスクを任せて別の作業に取り組む場合に便利です。いくつか Issue をリポジトリに登録し、Copilot に割り当ててみましょう。次のようなタスクを試せます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

cloud agent のハーネスを完了しました。各レッスンでは、次の操作を行いました。

- **このリポジトリにあるカスタム指示ファイルを確認**し、cloud agent の出力にどう影響するかを後で確認できるようにしました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用の**カスタム エージェントを作成して使い**、ハイコントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、作業中のアクセシビリティ セッションを監視して調整しました。
- **cloud agent のプルリクエストをレビューして反復**し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

cloud agent のハーネスを完了しました。他のハーネスでは、ここで学んだ内容を別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP の統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
