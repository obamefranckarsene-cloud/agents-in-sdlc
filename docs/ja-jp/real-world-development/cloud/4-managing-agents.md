---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、GitHub Copilot クラウド エージェントに、ユーザー エクスペリエンスの改善と機能追加に関する 3 つのタスクを依頼しました。クラウド エージェントは非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、クラウド エージェントに割り当てたタスクを管理するツールがいくつかあります。この Mission Control では、オープン PR のあるエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェント ページでクラウド エージェントのタスクを監視します。
- 進行中のセッションに、追加機能を依頼します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、一緒に実装するのが適切です。ハイ コントラストだけでなくライト モードも追加するよう、エージェントの作業に指示を出します。

## Copilot クラウド エージェントのタスクを確認する

Copilot クラウド エージェントに割り当てられたタスクの状態を確認しましょう。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用 API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した内容を確認します。コードのチェックアウト、作業環境の作成、MCP サーバーの設定、タスク完了に向けた各種操作などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実行方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業を監視できます。

## クラウド エージェントに追加の指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードを作業中の Copilot にライト モードの切り替えも依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。タイトルは Copilot が使った名前と作業状況によって異なります。

   ![Mission Control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで Copilot のアクティブ セッションに追加指示を出すダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼をワークフローに反映します。

この作業にも数分かかります。ここで少し休み、これまで学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot クラウド エージェントのタスクを監視し、案内する中心的な場所である Copilot エージェント ページについて学びました。Mission Control で進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、作業中のタスクに新しい指示を出して範囲を調整できます。

次の内容を確認しました。

- Mission Control とエージェント ページでクラウド エージェントのタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を作成している間に、[Copilot が作成したプルリクエストを反復的に改善します][next-lesson]。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
