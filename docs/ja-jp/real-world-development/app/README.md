---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot アプリ"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot アプリ](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションで、エージェント主導の開発を単一の作業用ワークスペースで実現します。並列エージェント セッション、切り替え可能なセッション モード、共有キャンバス、GitHub issue とプルリクエストのネイティブ管理機能を備えています。さらに、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまでプルリクエストを進める **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys を使った一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価に対象を絞った変更を加えてブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの命令を更新して、フィルターの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を確認します。
7. 品質保証 (QA) カスタム エージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバスが利用可能か確認し、Database Explorer を使った後、リポジトリに保存するトリアージ キャンバスを作成してテストします。必要な機能を利用できない場合は、その制約を記録し、読み取り専用の代替手段を使うか、まとめのレッスンに進みます。

ワークショップを分かりやすくするため、PR は 2 つ作成します。星評価の PR と、命令、スキルの更新、QA プロファイル、テストを含むフィルターの PR です。それぞれ更新済みの `main` から開始します。フィルターと品質ワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを学ぶ間に作業を引き継げます。最後のキャンバス演習は同じセッション内で行い、PR ワークフローを繰り返さず、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot アプリをインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 命令を確認して更新し、フィルターの作業に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行するチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザー確認 | Customize から MCP を構成し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター、命令、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ キャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを振り返ります |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot アプリは Codespace ではなく自分のコンピューターで実行されます。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使っている場合、管理者は **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI のポリシーとは別です。

## はじめる

**[前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
