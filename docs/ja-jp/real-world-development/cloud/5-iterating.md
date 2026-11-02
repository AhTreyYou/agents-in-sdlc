---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加に関する複数のタスクを GitHub Copilot に依頼してきました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の実装、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

何度も説明してきたように、生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は、監視を受けずに非同期でタスクを実行するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードを変更するブランチに対してのみ書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。これは想定された動作です。Copilot はデフォルトで、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが制限されています。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、Copilot が作業を終えるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc コメントなど、更新されたコードとドキュメントを確認します。具体的な変更内容は状況によって異なります。演習 2 の最初に確認したカスタム指示ファイルによって、TSDoc の doc コメント、TypeScript の規約、コメントヘッダーが追加されているか確認してください。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チームメンバーと同じように、プルリクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、必ず特定のテキストを使うよう指示することはできません。たとえば、次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc doc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントの変更を依頼するコメントを追加し、ユーザーと同じ方法で **@copilot** をメンションします。上記の例から選ぶか、コードベースに必要なドキュメントについて別の変更を依頼します。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、Copilot との反復作業を続けます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲームの表示について、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターがあるか確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace でターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent に変更を依頼して反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc doc コメントの追加を依頼したりできます（この作業は、カスタム指示を追加する**前**に割り当てたことを思い出してください）。前と同じように **Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを管理する JavaScript/TypeScript コード
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあるか確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace でターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の作業を支援したことを確認できました。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 作業をさらに依頼する

cloud agent には、バックログのタスクを任せて、その間にほかの作業に取り組む方法が効果的です。実践するために、リポジトリにいくつか issue を作成して Copilot に割り当てましょう。次のような例があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の演習を完了しました。各レッスンで、次の操作を行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認し、後から cloud agent の出力に反映されることを確認しました。**
- **Copilot cloud agent に issue を割り当て、環境の準備、計画、非同期の実行を確認しました。**
- **カスタムエージェントを作成して使用し、ハイコントラストとライトモードの切り替えを実装しました。**
- **Copilot agents ページをミッションコントロールとして使い、アクセシビリティセッションを監視して途中で指示を出しました。**
- **cloud agent のプルリクエストをレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## レビューと次のステップ

Cloud agent の演習は以上です。さらに学びたい場合は、次の演習も役立ちます。

- 🖥️ **[VS Code の演習](../../vscode/)** — IDE で Copilot Chat のエージェントモードと MCP 統合を体験します。
- 💻 **[CLI の演習](../../cli/)** — ターミナルから 2 つの変更をレビューし、Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent に戻る省略可能な `/delegate` ブリッジを使います。

自分のリポジトリでは、次の方法も試せます。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
