---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリング機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装したので、Copilot が使用した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示ファイルとパス別の指示ファイルがエージェントに適用される方法を確認する。
- コーディング標準に従うため、指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントには、コードの繰り返しではなく、意図や自明でない判断を記述する。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を付ける。`db` 引数を注入する場合は、その引数についても記述する。
- 再利用可能な Astro コンポーネントには `Props` 契約を説明するドキュメントを付け、関連コードが変わったらコメントも最新の状態に保つ。
- 既存のフォーマットと lint のガイダンスを維持する。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを進めるための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストを Copilot に提供できます。これにより、より適切な提案やコード スニペットを生成できるようになります。推奨するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体にも、特定の種類のファイルにも指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。このファイルには、ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを指定するファイルです。

> [!NOTE]
> その他の指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

作業を始めるために、スターター プロジェクトには指示ファイルが用意されています。変更を加える前に、内容を確認して影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示が適用されるファイルを指定する glob を設定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分割し、参考となる例を Copilot に示せます。

## チームの方針に合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ不足があります。コアの `copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. そのセクションの最後に、次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指示を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリング機能のコンテキストを保持したまま、新しい指示を読み込めるように、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   確認を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。次の操作を行いました。

- リポジトリの指示ファイルとパス別の指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行するため、再利用可能な `quality-checks` スキルを[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
