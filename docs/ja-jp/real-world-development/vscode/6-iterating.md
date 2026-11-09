---
title: "演習 6 - GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善するいくつかのタスクに GitHub Copilot と取り組んできました。エージェント モードでクライアントとサーバーにまたがるフィルタリングを追加し、Playwright MCP サーバーでブラウザーからその作業を手動でテストしました。その後、カスタム エージェントを使って、ハイコントラストとライト モードの切り替えというアクセシビリティ機能を実装し、作業中のセッションに追加の指示を送って範囲を広げました。次はローカルの作業を公開し、チームと同じ方法でレビューします。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。Copilot が生成したものを実際にレビューする必要があります。codespace からアクセシビリティの変更をプッシュし、プルリクエストを作成して、チームに共有する前に差分を確認しましょう。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と [演習 5][exercise-5] でアクセシビリティ用のカスタム エージェントを使って実装したハイコントラストとライト モードの切り替えは、コミット済みの変更として codespace にあります。チームでレビューできるように、ブランチをプッシュしてプルリクエストを作成します。

1. Codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更がコミット済みであることを確認します。演習 5 の未コミットの変更がある場合は、ステージして、`Add high-contrast and light-mode toggles` などの内容が分かるメッセージでコミットします。
4. **Publish Branch** を選んでブランチを公開します (または **...** メニュー → **Push** を使います)。
5. VS Code から新しいブランチを github.com で開くよう促されます。プロンプトを受け入れるか、手動でリポジトリに移動し、ブランチのバナーで **Compare & pull request** を選びます。
6. 分かりやすいタイトル (例: `Add high-contrast and light-mode toggles`) と、何をなぜ行ったかをまとめた短い説明を設定します。
7. **Create pull request** を選びます。
8. PR を開いたら、**Files changed** タブを選び、作業全体をレビューします。特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使い方。
   - ハイコントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript / TypeScript のコード。

9. **Conversation** タブに戻ります。
10. ワークフローが承認待ちになっている場合は、**Approve and run workflows** を選びます。

    ![「Approve and run workflows」ボタン](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ、成功したことを確認できます。

> [!TIP]
> アクセシビリティの作業について別の意見が必要ですか。PR コメントで `@copilot` に、たとえば「この PR に他の WCAG の問題がないかレビューしてください」や「キーボード操作の改善を提案してください」と依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## オプションの演習 — ローカルでさらに試す

IDE でエージェントと反復しながら作業することもスキルの 1 つです。身に付けるには繰り返しが必要です。VS Code から実行できる追加セッションのアイデアを紹介します。

- ゲームの詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- `src/lib/` のデータ アクセス ヘルパーに入力検証とエラー処理を追加する。
- アクセシビリティ エージェントの対象範囲を広げる。たとえば、サイト全体のキーボード フォーカス順を監査する。

## まとめ

おめでとうございます。VS Code ハーネスを完了しました。このラボでは、次の操作を行いました。

- **Playwright MCP を使って機能を手動でテストしました。** Playwright MCP サーバーを追加し、プルリクエストを作成する前に Copilot にブラウザーを操作させて、フィルタリング機能を確認しました。
- **エージェント モードでスタック全体にまたがる変更を調整しました。** 1 つのセッションで、クライアント、サーバー、テストにまたがるフィルタリング機能を追加しました。
- **カスタム エージェントを使いました。** エージェント選択メニューでアクセシビリティに特化したカスタム エージェントを選び、リポジトリにハイコントラスト モードを実装する様子を確認しました。
- **エージェント セッションを管理して調整しました。** 提案された変更をインラインでレビューし、必要なものを採用し、ライト モードの追加依頼でセッションを拡張しました。
- **プルリクエストで作業を完了しました。** ローカルの作業を公開し、チームと同じ方法で全体をレビューしました。

## レビューと次のステップ

これで必須の VS Code ハーネスは完了です。ここでワークショップを終了できます。

Copilot のエージェント機能についてさらに学ぶ場合は、他のハーネスで異なる環境から関連するシナリオに取り組めます。

- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、コンテキストを使った引き継ぎを活用し、ターミナルからレビュー済みの変更を 2 つリリースします。
- ☁️ **[Cloud agent ハーネス](../../cloud/)** — cloud agent への Issue の割り当て、agents ページでのセッションの監視、プルリクエストの非同期な反復に取り組みます。

ここで始めた作業をさらに発展させることもできます。[awesome-copilot][awesome-copilot] には、自分のプロジェクトに応用できる指示ファイル、カスタム エージェント、スキルが豊富にあります。

オプションの拡張として、[オプション: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェント モードを使う][agent-mode]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
