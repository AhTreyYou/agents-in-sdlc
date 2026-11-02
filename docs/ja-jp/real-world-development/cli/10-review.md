---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを体験しました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、フィルタリングの issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge による PR のマージを承認しました。
- コンテキスト、モデル、共有、省略可能な cloud agent への委任に使えるスラッシュコマンドを確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` から作成した各ブランチで、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価であることを示す表示を、ゲームカードに追加しました。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加しました。

ワークショップを簡潔にするため、フィルタリングの計画から PR の作成まで、同じ会話とブランチで作業し、変更を 1 つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは、ユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告します。QA エージェントは、その結果を要件やテストカバレッジの確認と合わせて PR 前にレビューします。

追加するテストは、実際に不足しているテストを補うものである必要があります。カバレッジに不足がなく、QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の根拠をレビューしてください。変更を加えた場合は、影響を受ける検証を再実行してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは再利用可能なタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方法を検討するとき、**Interactive** は小さな変更を進めながら状況を確認するとき、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

ツールや環境と同様に、コンテキストも重要です。何を作るのか、なぜ必要か、どのように実装するのかを明確に説明すると、結果が大きく変わります。

## さらに試す

主なワークフローを完了しました。Copilot CLI の便利な追加機能を紹介します。

- `/review` を使って、コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck` を使って問題を相談し、別の視点から意見をもらいます。
- `/fleet` を使って独立したサブタスクを並行して調整します。
- `/worktree` を使って別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のプロジェクト、何年も作ろうと思いながら手を付けていなかった小さなアプリなどで試してみましょう。チームと学んだことを共有し、互いの経験から学んでください。そして、ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張したい場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
