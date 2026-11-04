---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの根拠を評価しました。
- フィルター変更全体をレビューし、フィルター PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、それぞれ更新済みの `main` から分岐したブランチ上で行う、2 つの PR の節目があります。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価状態を明示して表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、1 つの PR にまとめました。

## 異なる種類の検証

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA はこれらの結果と、PR を開く前の要件およびテスト カバレッジのレビューをまとめました。

テストは実際の不足を補うものであるべきです。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は影響を受ける根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の方向性を決めます。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **モードとモデル** を選びます。構築前に方針を検討するときは **Plan**、範囲を絞った変更でやりとりを続けたいときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを選び、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、結果が大きく変わります。

## さらに確認する

中心的なワークフローを学びました。Copilot CLI には、他にも次のような機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot クラウド エージェントにタスクを送信します。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。実運用のコード、趣味のコード、何年も考えていてまだ作れていない小さなアプリなどに活用してください。学んだことをチームと共有し、チームの知識から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリ ワークショップ][app]、または[Copilot クラウド エージェント ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
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
