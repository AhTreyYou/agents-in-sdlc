---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行したい場合は、Copilot がその指示を参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装できたので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントには、コードの繰り返しではなく、意図や自明ではない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。該当する場合は、差し替え可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を説明し、関連するコードの変更時にはコメントも更新します。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示では、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を指定できます。コーディング規約、ライブラリ、追加してほしいコメントなどを指定することで、関連性の高い提案やコードスニペットを得やすくなります。リポジトリ全体や、特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての** Copilot リクエストに送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイル形式、タスクに関する指針を記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、作業を始めやすいように、いくつかの指示ファイルがあらかじめ用意されています。変更を加える前に、どのような指示があるかを確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。これらの指示は、このリポジトリで Copilot を使うすべての作業に適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示を小さく再利用しやすいファイルに分け、Copilot が従う例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ足りない部分があります。生成された TypeScript に TSDoc コメントが追加されるよう、基本となる `copilot-instructions.md` ファイルを変更しましょう。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを見つけます。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルタリングのコンテキストを保ったまま会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがあり、コードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を確実に守るため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な [`quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
