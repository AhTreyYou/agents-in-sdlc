---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使ううえで、コンテキストは重要です。タスクを特定の方法で実行したい場合や、Copilot に事前知識を与えたい場合は、その情報を参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。コード生成に関するガイダンスを提供します。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリングに必要な最初のデータ部分（発行者ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新とフィルタリング機能を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は決定論的ではありません。この演習では、一字一句同じ出力を得るのではなく、指示の更新後に動作がどのように変化するかを確認します。

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

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成しましょう。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して、同じプロンプトをもう一度送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べて、コードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、発行者の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約に基づいています。
7. 提案されたコードに TSDoc doc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。ここにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、先ほど生成されたヘルパーには doc コメントがありませんでした。
3. 次の Markdown 行を既存の標準の直後に追加し、ファイルコメントヘッダーと TSDoc doc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` を使って全体に適用する指示を設定したり、特定の言語、ファイル形式、タスク向けの `*.instructions.md` ファイルを使ったりできます。今回追加した doc コメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し直し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準に従うよう Copilot をガイドし、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。指示を更新してブランチにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーが更新されていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新と Copilot がヘルパーの基盤として変更したファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を探し、関連付けた PR を作成してからマージするよう指示しましょう。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を実行します。プロジェクトのテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインが求められることがあります。画面の指示に従って許可してください。後の演習で、この接続の仕組みである Model Context Protocol について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成して、指示の更新と発行者ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探し、PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- コード生成のプロンプトを送り、*既存の*指示で Copilot が何を生成するかを確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準を採用することを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は[エージェントモード][next-lesson]を使い、Copilot にコードベース全体で新機能を追加させます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
