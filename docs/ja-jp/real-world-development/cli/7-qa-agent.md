---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、その機能を明確な QA プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが命令、スキル、MCP ツールを使う方法を確認します。
- 再利用できる品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター issue に照らした結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー動作を一貫して確認したいと考えています。カスタム エージェントを使えば、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルでは、エージェントの目的、命令、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を記述します。`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能をどう使うかを示します。これらの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。プロファイルでは、実行するチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、プロファイルをレビューし、意図した QA の流れと制約が反映されていることを確認します。これにより、検証だけを依頼したのに不完全または範囲の広すぎるエージェントが機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの命令を読み、要件を起点とし、リポジトリの命令に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に許可を求め、コミットやプルリクエストの作成を行わないことを確認します。
5. いずれかの役割や制約が欠けている場合は、続ける前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成したら同じフィルター会話を再開し、QA を選択します。そうすれば、issue と計画の決定を引き続き参照できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトを使って、機能のレビューを QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割を追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが命令、スキル、MCP ツールを使う方法を確認しました。
- 再利用できる品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター issue に照らした結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、それらをまとめて機能 PR を作成し、Agent Merge を使います。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
