---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要があります。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、CLI に組み込まれている機能を超えるツールやサービスを Copilot に提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストや E2E テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように作業中の Web サイトを Copilot が使えるようにすれば、変更が期待どおりに動作する確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部サービスとリアルタイムに通信し、最新情報へのアクセスや操作を行えます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

他にも多くの MCP サーバーがあり、GitHub はエコシステム内での発見や貢献を促進するために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、他の依存関係と同様に扱ってください。使用前にソース コードをレビューし、発行元を確認して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

これまでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、MCP サーバーの使用を Copilot に指示しなくても自動的に判断されます。使用するツールが分かっている場合は、明示してもかまいません。結果が安定し、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を実行する様子を確認します。
3. 報告を読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を調べました。

次は、スキルとブラウザー ツールをまとめた[QA カスタム エージェントを作成し][next-lesson]、専門的な役割として利用します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
