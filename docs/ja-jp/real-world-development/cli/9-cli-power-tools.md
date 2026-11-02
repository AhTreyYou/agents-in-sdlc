---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、省略可能な cloud agent への委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI には対話に使える多くのスラッシュコマンドがあります。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか紹介します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える省略可能なコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えました。次に、コンテキスト管理、モデルの切り替え、セッションの共有、省略可能な [Copilot cloud agent][about-cloud-agent] への委任など、追加機能をいくつか紹介します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキストウィンドウが上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話が短い場合は、大きな違いがないことがあります。

> [!NOTE]
> Copilot CLI は、コンテキストウィンドウがいっぱいになると自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業内容や知見を共有すると、AI ツールの活用方法を互いに学べます。`/share` コマンドを使うと、Markdown または HTML ファイルへのエクスポート、共有リンクの作成、GitHub gist の公開ができます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown へのエクスポートを行う `/share file` など、目的に合った方法を選択します。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を分割または委任する

ワークショップの主な内容は完了しました。Copilot CLI には、大規模なタスクに役立つコマンドもあります。

- `/fleet` を使うと、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` を使うと、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` を使うと、Copilot cloud agent にタスクを送信できます。cloud agent は非同期に作業し、プルリクエストを作成することがあります。

追加の worktree やリモート作業が発生するため、これらのコマンドは省略可能です。試す場合は、新しい範囲の明確なタスクを始め、通常のワークフローで結果をレビューしてください。非同期エージェントによる作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える省略可能なコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は、学んだことを[振り返り、今後の学習方法を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
