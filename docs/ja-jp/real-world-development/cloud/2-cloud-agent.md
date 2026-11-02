---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな問題があります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるよう、ドラフトのプルリクエスト (PR) を作成します。手間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次の操作を行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 明確に範囲を定めた issue の重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しておらず、実際にはドキュメントがまったくありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc の doc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを表示したいという要望があります。現時点では完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度の高い別の issue もあります。

このようなタスクは優先順位が下がりやすいため、Copilot cloud agent に任せるのに適しています。エージェントに非同期で作業させている間、開発者はほかの作業に取り組み、後から成果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を記録するドラフトのプルリクエストを作成し、環境を準備して、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整しましょう。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその役割を確認しました。カスタム指示の演習をすでに完了していれば、`.github/copilot-instructions.md` が機能することを確認できたはずです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを参照して、後でその影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc doc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるとき、作成されたプルリクエストで TSDoc doc コメント、TypeScript の規約、コメントヘッダーが使われていることを確認してください。後の演習で PR をレビューするときにも、改めて説明します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成する際は、通常、必要な環境やセットアップスクリプトを準備して、状態を整えます。Copilot にタスクを割り当てる場合も同じです。cloud agent はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップワークフロー][setup-workflow]を作成すると、cloud agent が作業を始める前に実行するツールや依存関係を指定できます。このラボでは、学習をスムーズに進められるよう、事前に設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを cloud agent が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot は自身の操作に専用のトークンを使います。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドやテストでデータを利用できるよう、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub Actions ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと理解していても、多くのプロジェクトでは情報が古かったり、ドキュメントがまったくなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に依頼するのに適した issue です。エージェントがバックグラウンドで必要なドキュメントを作成します。後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が作業項目のリストを作成します。

> [!NOTE]
> Copilot が PR に作業項目を表示するまで、数分かかる場合があります。前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. 作業項目と、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターが表示されない場合は、ウィンドウを更新してください。

16. セッションをリアルタイムで確認します。Copilot がコードを調べて状態を理解し、方針を考えて判断した後、コードを作成する様子を確認できます。

作業には数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行して、ほかの作業に取り組めるようにすることです。この機能を活用するため、cloud agent にもう 1 つのタスクを割り当ててから、アプリケーションの機能追加にも取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前に説明したように、GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業は、時間がかからないとしても、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを確認できます。

![issue で Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するので、完了まで積極的に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントを作成して使う方法を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期で作業を実行できます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

このレッスンでは、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 明確に範囲を定めた issue の重要性を学びました。
- issue を Copilot に割り当てました。

cloud agent が作業を続ける間に、カスタムエージェントの作成と使用に進みましょう。[Copilot cloud agent では MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで学んだカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
