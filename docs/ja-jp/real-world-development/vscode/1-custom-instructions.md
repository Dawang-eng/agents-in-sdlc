---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要があるなら、その指針を Copilot が参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが、コード生成の指針を提供する[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- 現在の指示がある状態で、フィルタリング用の最初のデータ スライス (publishers ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードに標準が適用される様子を確認する。
- 指示の更新とフィルタリングの基盤を `main` にコミットし、Copilot が次の演習で新しい指針を使えるようにする。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどのように変わるかを確認します。生成結果を文字単位で同じにすることが目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データ レイヤーには必ずユニット テストを用意する。
- UI はダーク モードで、現代的な見た目にする。
- コードのドキュメントには TSDoc の doc コメントを使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメントを追加する。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを進めるための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に提供できます。これにより、より適切な提案やコード スニペットを生成できるようになります。推奨するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体にも、特定の種類のファイルにも指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot とのチャットや CLI の多くのリクエストに関係する、技術スタック、構築中のもの、ベスト プラクティスなど、プロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript などの特定の言語や、UI コンポーネントまたはユニット テスト作成などのタスクに関する指針を設定するファイルです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターが付いた `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、別の規格を使う指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を確認してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトには代表的な方法が示されています。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、構築中のもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに関する指針を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例も示します。

指示ファイルの作り方は 1 つではありません。AI の使い方と同様に、プロジェクトで試しながら適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境で操作方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各レッスンでは、関連する操作方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの短い説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートからの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに指示を適用します。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、コード生成の参考例も示せます。ここにあるパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で TSDoc の doc コメントとファイル先頭のコメント ヘッダーを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選んで新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使った状態でプロンプトを送ります。その後、指示を更新し、同じプロンプトをもう一度送ります。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. Copilot にヘルパーの配置場所を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択メニューで **Agent** を選び、Copilot がファイルを編集できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどにコードの変更を適用します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc の doc コメントとファイル先頭のコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する場合があります。その場合も問題ありません。この演習では、指示を更新した後に動作が一貫して改善することを確認します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コード提案の質を高めるため、コーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の項目を追加します。ファイル ヘッダーと TSDoc の doc コメントを Copilot に指示する内容です。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体に適用する指示を、`*.instructions.md` には言語、ファイルの種類、タスクに固有の指示を設定できます。今回追加した doc コメントのルールのようなプロジェクト標準は、リポジトリ レベルの指示ファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んでコンテキストをクリアします。
2. Copilot が適切なファイルに集中できるよう、`src/lib/publishers.ts` をもう一度選びます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に、次のような TSDoc コメントが付いていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも他のアセットと同様に、ソース管理で管理します。指示の更新をコミットして、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、Issue に関連付けたプルリクエストを作成します。手動で操作する代わりに、GitHub ツールに接続済みの Copilot に Issue を探し、PR を作成してマージするよう依頼できます。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上で操作します。プロジェクト テンプレートには GitHub のツールがあらかじめ接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従ってください。後の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** を選びます。
3. 指示ファイルの更新と新しい publishers ヘルパーの基盤を説明する PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub の操作を許可するよう求めたら、コマンドを確認して適切に **Allow** を選びます。
5. PR が作成されたら、PR を `main` にマージし、ローカル ブランチも `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub の操作やシェル コマンドの実行を許可するよう求めたら、内容を確認して適切に **Allow** を選びます。

これで GitHub Copilot のサポートを受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

指示ファイルから Copilot にコンテキストが伝わる方法を確認し、Copilot Chat を VS Code で使って次の操作を行いました。

- 既存の指示がある状態でコード生成のプロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再度送り、更新した指示が提案コードに反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は、[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体へ新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに関する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
