---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びながら、Tailspin Toys に 2 つの変更を加えて検証し、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、エージェント型コーディング アシスタントとして GitHub Copilot をターミナルで利用できるようにします。コマンド ラインからコードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys を使った一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、使い方を確認します。
2. 星評価の変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの命令を更新して、フィルターの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. 品質保証 (QA) カスタム エージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター変更全体をレビューし、Agent Merge を使ってフィルター PR を処理します。
9. コンテキスト、モデル、共有、任意のクラウド委任に使う便利なスラッシュ コマンドを確認します。

ワークショップを分かりやすくするため、PR は 2 つ作成します。星評価の PR と、命令、スキルの更新、QA プロファイル、テストを含むフィルターの PR です。各ツールを学ぶ間に作業を引き継げるよう、フィルターと品質ワークフローでは 1 つの会話とブランチを使います。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、使い方を確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認して更新し、フィルターの作業に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザー確認 | CLI で MCP を構成し、フィルターの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選択し、最終検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、クラウド エージェントへの任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、その他のリソースを振り返る |
| [任意: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれます。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使っている場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## 始める

**[前提条件から開始 →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
