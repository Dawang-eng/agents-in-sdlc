---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を立ち上げます。次のレッスンでは、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の操作を行います。

- template から Tailspin Toys プロジェクトの自分用コピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[template リポジトリ][tailspin-template]からコピーを作成します。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. **Use this template** を選び、続けて **Create a new repository** を選んで自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる organization に新しいリポジトリを作成します。
4. 後でワークショップで使うため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこの Issue を使って作業するため、自分で起票する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリの指示、アプリケーション コード、テスト、`quality-checks` スキル、演習で使うバックログが含まれています。以前作成したコピーを使う場合は、必要なファイルが揃っているか進行役に確認してください。

## Codespace を作成する

次は、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールをサポートするフル機能のエディターを利用できます。

1. 新しく作成したリポジトリに移動します。
2. **Code** を選びます。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによってプロジェクトの依存関係、Playwright Chromium、ローカル データベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と表示された場合は **Trust Folder & Continue** を選びます。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選び、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

セットアップが完了しました。このレッスンでは、次の操作を行いました。

- template から Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は、Codespace に[GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
