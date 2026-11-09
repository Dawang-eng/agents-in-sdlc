---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づいてフィルタリングを実装し、Interactive で結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。しかし、大きな変更には、より確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切なものを適切な方法で構築できます。このレッスンから、エージェントを活用した標準的な開発プロセスをいくつかのレッスンにわたって進めます。Issue から新機能を実装し、コードの妥当性と機能の動作を確認したうえで、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常、ファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため、手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot の新しい会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- ポート転送したブラウザーで機能を手動で検証する。

この機能の開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリと発行者でゲームを絞り込める必要が出てきました。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に合意しておきたい詳細があります。Plan モードで方針を決めた後、範囲を限定して Autopilot に実装を任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、ソフトウェア開発の基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 実装する内容を説明した Issue から始めます。
2. 構築する内容を計画します。
3. コードを実装し、レビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、通常はここに示した流れを調整したものになります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリング機能の作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画を立てるのがお勧めです。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 先ほど追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、適切な方法を判断しながら計画を作成します。

3. フィルタリング機能の動作について質問されたら、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読み返します。
6. 決めた方針と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画ができたら内容を確認し、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot が完了したと判断するまでタスクを反復して進めます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の意味を持つ選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、検証が完了したと判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードが生成されたら、他のコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容と照らし合わせます。
4. コードを確認したら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードの準備ができました。次に、アプリを起動して Codespaces のポート転送を使い、ブラウザーでサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選びます。
3. カテゴリによるフィルタリング、発行者によるフィルタリング、計画で決めた組み合わせを試します。
4. リセット時や結果がない場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- ポート転送したブラウザーで機能を手動で検証しました。

次は、コード生成がドキュメント化されたプラクティスに従うように、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
