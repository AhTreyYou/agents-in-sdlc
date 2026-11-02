---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このミッションコントロールでは、開いているプルリクエスト (PR) に関連するすべてのエージェントタスクを確認できます。実行された操作を確認したり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加しているところなので、ハイコントラストに加えてライトモードも実装するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されるタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する
   - ゲームを変更するための API を作成する
   - Web サイトにハイコントラストモードを追加する
3. 実行中のタスクを 1 つ選択し、Copilot が実行した操作を確認します。次のような操作が含まれます。
   - リポジトリをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーをセットアップする
   - 割り当てられたタスクを完了するためのさまざまな手順を実行する

> [!NOTE]
> 表示される手順は、Copilot の作業状態や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) の領域も確認します。ここでは、監視対象の PR と変更されたファイルを確認できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイコントラストモードを作業中の Copilot にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。表示される正確なタイトルは、Copilot が付けた名前や作業状態によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで、数分間セッションを確認します。下記のようなメッセージが表示されたら、作業が始まったことが分かります。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションを誘導するダイアログを示したエージェントページの画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れることを確認します。

## Copilot の作業を待つ

前と同じように、更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼を作業に取り入れます。

今回も完了まで数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、指示を出す中心的な場所である Copilot agents ページについて学びました。このミッションコントロールでは、実行中または完了したタスクを追跡し、作業内容を確認したり、進行中のセッションを調整したりできます。

このレッスンでは、次の操作を行いました。

- ミッションコントロールとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能の作業を続ける間に、Copilot が作成したプルリクエストを[反復的に改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
