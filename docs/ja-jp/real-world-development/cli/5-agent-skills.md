---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードが動作するか手動で検証し、命令ファイルを使って標準に従わせました。テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルは、こうした操作を適切に実行する方法を Copilot が理解するために役立ちます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースがまとめられており、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` ファイルで構成されます。Markdown には、スキルの内容を定義する `name` と `description` のフロントマター、機能の概要、呼び出すタイミングのガイダンスを記載します。スキルから使うスクリプトなどのリソースを、サブフォルダーに含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるリポジトリ資産として `.github/skills` に配置するか、ユーザー用の `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に用意した `quality-checks` スキルを確認しましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。description によって、Copilot がスキルを呼び出すタイミングを判断できることを確認します。
3. 命令を読み、テストと lint の手順を Copilot に示す方法を確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを実行できます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

どのチェックを実行したか、成功したか、ツールが実際に何を報告したかが分かるレポートに改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

更新したスキルを試してみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを読み、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズし、実行しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

フィルター機能とともに、この変更を機能 PR に含めます。次は、[Playwright MCP サーバー][next-lesson]を使って Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考資料です。追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
