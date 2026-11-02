---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。特定の方法でタスクを実行したい場合や、Copilot に事前知識を与えたい場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub issue を非同期で処理するため、リアルタイムで影響を並べて示すのが難しいためです。この演習の後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ずユニットテストが必要です。
- UI はダークモードにし、現代的な印象にする必要があります。
- コードのドキュメントには TSDoc の doc コメントを使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発方針に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示では、Copilot がコーディングスタイルや要件を理解するためのコンテキストと設定を指定できます。コーディング規約、ライブラリ、追加してほしいコメントなどを指定することで、関連性の高い提案やコードスニペットを得やすくなります。リポジトリ全体や、特定のファイルを対象にした指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての** Copilot リクエストに送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、通常のチャットや CLI のリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象にできます。たとえば、TypeScript などの言語や、UI コンポーネントやユニットテストの作成に関するガイドラインを指定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、他の標準形式の指示ファイルにも対応しています。詳細は [Copilot が対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。ただし、このサンプルプロジェクトの例から、一般的な方針を確認できます。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに必要な指示を記載します。
- 自然な言葉で、明確なガイドラインとコード例を示します。

指示ファイルの書き方に決まった方法はありません。AI の使い方と同じように、実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。手順は利用する環境によって異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各環境のレッスンで、該当する機能を紹介します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認してください。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらの指示は Copilot を使ったあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob が設定されています。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を小さく再利用しやすいファイルに分け、Copilot がコードを生成するときに参考となる例を示せます。ここに記載されたパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc コメントとファイルコメントヘッダーに関するルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 か所だけです。コード内のドキュメントには TSDoc の doc コメントとファイルレベルのコメントヘッダーを使うという標準を追加します。この演習の後半で issue を割り当てると、cloud agent がこの標準を参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、変更を整理してコミットするためにもブランチを使います。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。ここにはプロジェクトのコーディング標準が記載されていますが、生成コードに doc コメントが含まれなかった理由のとおり、コード内のドキュメントに関するルールはまだありません。
3. 次の Markdown 行を既存の標準の直後に追加し、ファイルコメントヘッダーと TSDoc の doc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` を使って全体に適用する指示を設定したり、特定の言語、ファイル形式、タスク向けの `*.instructions.md` ファイルを使ったりできます。今回追加した doc コメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は、issue の対象ブランチから指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を参照できるよう、変更を `main` に反映してください。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。これで、次の演習で cloud agent が issue に取り組むときに、この指示が使われます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの作業方法を体験できるよう、ここではブランチを使用しています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。この演習の後半で cloud agent が作成するプルリクエストをレビューするときに、この標準が使われていることを確認できます。

次は、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
