---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に扱うデスクトップ アプリです。Issue やプルリクエストにすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、自分のコンピューターで GitHub Copilot app を使い、Astro で構築された Tailspin Toys アプリを更新します。始める前に、ローカル環境に Node.js がインストールされていることを確認し、次に Copilot app をインストールします。

このレッスンでは、次の操作を行います。

- プロジェクトのテストをコンピューターで実行できるように Node.js をインストールする。
- Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成する。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築させ、Tailspin Toys のテスト スイートをローカルで実行します。そのためには [**Node.js**][nodejs] が必要です。プロジェクトに必要なランタイムは Node.js だけです。現在の **LTS** リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、インストールされている Node.js のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合や、更新が必要な場合にのみ、これらの手順を実行してください。

4. [Node.js のダウンロード ページ][node-download]を開きます。
5. 使用しているオペレーティング システム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選びます。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナル ウィンドウを開きます。
8. 新しいターミナルで次を実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールした Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントからも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了してから再起動してください。セッション内でどちらかのコマンドが失敗する場合は、続行する前に[セッション環境のトラブルシューティング][session-environment]を行ってください。

> [!IMPORTANT]
> E2E チェックには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree を準備するときは Tailspin Toys リポジトリの README に従い、インストールを求められたら、承認する前に内容を確認してください。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][template-repository]からコピーを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。次のレッスンでアプリに接続します。

1. 新しいブラウザー ウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選びます。続けて **Create a new repository** を選び、自分用のコピーを作成します。

    ![「Use this template」ボタンのドロップダウンで「Create a new repository」が選択されている画面](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。

    ![テンプレートに github-samples/tailspin-toys が設定され、リポジトリ名が入力された「Create a new repository」フォーム](../../../_images/app-0-create-repository.png)

4. ラボの後半で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通じてこの Issue を使って作業するため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、quality-checks スキル、既存のキャンバス拡張機能が含まれています。ワークショップではスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルがあるか進行役に確認してください。

## まとめと次のステップ

準備が整いました。このレッスンでは、次の操作を行いました。

- プロジェクトをコンピューター上でビルドしてテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
