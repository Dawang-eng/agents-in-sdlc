---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、作業用の [codespace][codespaces] を立ち上げ、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

これから作成するコード用に、[template][template-repository] からリポジトリのコピーを作成します。新しいリポジトリにはラボに必要なすべてのファイルが含まれており、演習を進める間はこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] に移動します。
2. ラボ用リポジトリのページで **Use this template** ボタンを選び、自分用のコピーを作成します。次に **Create a new repository** を選びます。

    ![「Use this template」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環として参加している場合は、メンターの案内に従ってください。それ以外の場合は、GitHub Copilot を利用できる organization に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後でラボ内で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備は完了しています**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこの Issue を使って作業します。自分で Issue を起票する必要はありません。

## Codespace を作成する

次は、codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] はクラウドベースの開発環境です。ブラウザーから直接コードを作成、実行、デバッグできます。複数のプログラミング言語、拡張機能、ツールをサポートするフル機能の IDE を利用できます。

1. 新しく作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選びます。

    ![「Code」ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと早く準備できます。その間に GitHub Copilot の機能を確認しておくこともできます。次の演習から使い始めます。

> [!CAUTION]
> 後の演習で codespace に戻ります。いまはブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは、codespace またはローカルの [dev container][dev-containers] 内で実行するように作られています。どちらの環境にも、必要な前提条件があらかじめインストールされています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選びます。VS Code が codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。Visual Studio Code の **Extensions** ビューを開いて **GitHub Copilot Chat** を検索し、**Install** を選んでもかまいません。

拡張機能をインストールした後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選びます。
3. Copilot Chat ウィンドウに `Hello world` などのメッセージを入力し、Enter を押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証が完了すると、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用のリポジトリのコピーを作成し、コードを書くための codespace を作成しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
