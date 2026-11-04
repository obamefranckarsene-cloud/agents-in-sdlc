---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、クラウド エージェントはバックグラウンドでリポジトリを調べ、変更を行い、プルリクエストを開きます。その間、別の作業を続けられます。

演習では、まずクラウド エージェントが従うカスタム命令を追加し、GitHub issue を割り当てて実装を依頼します。カスタム エージェントを使って方針を定め、エージェントのダッシュボードからセッションを監視して案内し、最後にプルリクエストをレビューして結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | クラウド エージェントが従うカスタム命令を追加する |
| [2. クラウド エージェント][ex2] | 非同期エージェント | GitHub issue を Copilot クラウド エージェントに割り当てる |
| [3. カスタム エージェント][ex3] | 特化型エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの管理][ex4] | 監視 | エージェント セッションを監視して案内する |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善し、次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次の項目を確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは組織で Copilot クラウド エージェントが有効になっていること

> [!TIP]
> 有料プランを利用していない場合でも、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コード レビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウド エージェントは、Copilot Free 以外のすべてのプラン、つまり **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 始める

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
