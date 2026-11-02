---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探し、変更し、テストを実行して、問題をデバッグし、更新し忘れたファイルを見つけて、さらに変更を加えます。作業は際限なく続くことがあります。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業します。開発者と同じように、既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに作られています。エージェントモードは IDE 内でタスクを実行し、すばやいフィードバックと対話を可能にします。Cloud agent はチームメンバーのように非同期で作業し、issue とプルリクエストを通じて連携します。

この演習では、次の操作を学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数のファイルにわたって変更する方法。
- GitHub Copilot エージェントモードが UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行者で絞り込めるようにします。前の演習では発行者ヘルパーを追加しました。次は、Copilot エージェントモードで残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。サイト全体が単一の Astro アプリであり、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接照会します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace では、ターミナルに表示された localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを公開する codespace のプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **Home Page**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに、issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は複数のファイルにわたって自律的に変更を計画、実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの開発時には Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、快適に学習できることを期待しています。ただし、LLM は確率的に動作するため、説明と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを与える指示ファイルを確認します。[ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する際の考え方が記載されています。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するための全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - 次のようなファイル形式ごとの指示ファイルへのリンク
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を管理しやすい単位に分け、例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリング機能を完成させるには、少なくとも 3 種類の更新が必要です。

- データ層 (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

また、コードベースにマージする前にテストを実行し、成功することを確認する必要があります。Copilot エージェントモードに作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルに引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定します。データ層のヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行します。タスクやコードベースの複雑さによっては、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。作業が完了するとすべての変更に **Keep** または **Undo** が表示されるため、進行中に選択する必要はありません。

7. Copilot の指示に従い、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクを完了し、エラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動しているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。この演習では、次の操作を学びました。

- GitHub Copilot エージェントモードで、UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを特定し、複数のファイルにわたって変更する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションを実装するよう依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページネーションの実装方法を検討します。
- 希望どおりの実装ができるよう、プロンプトに必要なガイダンスを含めます。
- Copilot に変更を依頼してコンテキストを追加しながら、複数回やり取りする場合があります。Copilot を使うときの通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
