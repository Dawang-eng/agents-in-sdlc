---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に事前情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を確認するのは困難です。cloud agent が作成したプルリクエストをこのハーネスの後半でレビューするときに、指示ファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI へのほとんどのリクエストに関係する、プロジェクトレベルの情報を記載します。たとえば、技術スタック、構築中のものの概要、ベスト プラクティス、全体に適用するガイダンスなどです。
- `.github/instructions/*.instructions.md` は、TypeScript などの特定の言語や、UI コンポーネントやユニット テスト作成などのタスクに関するガイドラインを指定するファイルです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターが付いた `*.instructions.md` ファイルの両方を使ってコードを生成します。
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
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、コード生成の参考例も示せます。パスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で TSDoc の doc コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリ全体に適用する標準を追加する

次に行う編集は、TSDoc の doc コメントとファイル単位のコメント ヘッダーをコードに追加するためのルールです。後の演習で cloud agent が Issue に取り組むとき、このルールを読み取ります。

編集の前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは編集内容をきれいにコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、Copilot にプロジェクト レベルの情報を提供するファイルです。コード提案の質を高めるため、コーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加します。ファイル ヘッダーと TSDoc の doc コメントを Copilot に指示する内容です。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全体に適用する指示を、`*.instructions.md` には言語、ファイルの種類、タスクに固有の指示を設定できます。今回追加した doc コメントのルールのようなプロジェクト標準は、リポジトリ レベルの指示ファイルに記載します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更は `main` にマージしておく必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で cloud agent に作業を割り当てると、更新した指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成する手順を省略し、`main` に直接コミットできます。ここでは実際のプロジェクトに近い手順を体験するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot にコンテキストが伝わる方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot 用の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
